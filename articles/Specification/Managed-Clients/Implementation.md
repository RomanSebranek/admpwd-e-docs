# Implementation
## Packaging and basics
Management runtime is implemented as follows:
* On Windows Nano Server:
  * Single executable **AdmPwd.E.Client.exe**: management service executable  
  *Note*: Management Service name is "AdmPwd.E.Client"
  * **Client.Nano.man**: event manifest that registers executable as event provider for Event Log service
* On Other Windows platofrms:
  * Single executable **AdmPwd.dll**: GPO CSE extension loaded autimatically as needed by GPO Framework
  * **Client.man:** event manifest that registers executable as event provider for Event Log service
  * CSE publishes the following entry points:
    * **ProcessGroupPolicy** -- It is main entry point for Group Policy framework. This entry point implements <a href="http://msdn.microsoft.com/en-us/library/aa374377.aspx" target="_blank" rel="noopener noreferrer">ProcessGroupPolicy()</a> callback
 	* **DllRegisterServer** -- Can be used for manual registration of CSE with GPO framework during the CSE installation/upgrade in case that it is not possible to use MSI installer for installation
 	* **DllUnregisterServer** -- Can be used for manual deregistration of CSE from GPO framework during the uninstallation process of CSE in case that it is not possible to use MSI installer for installation

## Logic of processing

1. CSE connects to Active Directory; to the computer object managed machine it is running on
2. CSE then reads the value of attribute `ms-MCS-AdmPwdExpirationTime`. This attribute stores the expiration time of current password  
3. Runtime then performs check for password validity based on configured policies and password age:  
    * *Password was never changed*: When the attribute is empty, password was never changed, so management runtime knows it is the time to reset the password  
 	* *Password has not expired yet*: When the timestamp is not older than current time, password has not expired yet, and management runtime does not perform any other operation and finishes processing  
 	* *Password has expired*: When the timestamp is older than current time, management runtime knows it is the time to reset the password  
    * *Password expiration is too long*: When configuration requires password to comply with maximum age restriction, management runtime compares expiration time with maximum age of password specified in GPO. When expiration is longer than maximum age, management runtime knows that it’s time to change the password and reset the password age  
    * *Password was manipulated with*: When configuration requires protection against password changes outside of solution, management runtime reads age of managed local administrator account and compares it with expected age, given by last password change performed by management runtime. When age is other than expected, password is considered invalid and management runtime knows it’s time to change it
4. When password needs to be Changed, management runtime detects the local Administrator account to manage (either via name configured using GPO or via well-known SID) and connects to it  
5. Then management runtime generates new password according to required criteria (length and complexity) - using cryptographically strong algorithms
6. In case that solution is configured to store the password encrypted in AD, management runtime loads encryption key from registry and uses it to encrypt the password. Management runtime then converts encrypted blob to Base64 string
   * Encryption key is delivered to registry by either DSC (on Nano server) or by GPO (on other platforms)
7. Then management runtime reports new password (plain text or Base64 encoded encrypted blob) and password expiration timestamp to Active Directory, to the following attributes of computer account for machine it runs on:
   * `ms-MCS-AdmPwd`: password itself
     * either plaintext password
     * or Base64 string containing encrypted password, prefixed by ID of the key used for encryption  
    *Format*: `<keyID>:<space><Base64>`  
    *Example*: `1: ZmwKf34lH1/+NsjIWSfKQSb4H…`  
    * `ms-MCS-AdmPwdExpirationTime`: timestamp of current time plus configured age of password, in FILETIME format (64-bit integer), in UTC  
 	* `ms-MCS-AdmPwdHistory`: in case that configuration requires maintenance of password history, timestamp of current time (Directory string) plus password as reported to ms-MCS-AdmPwd attribute  
    *Format*: `<timestamp>:<space><value of ms-MCS-AdmPwd>`  
    *Example*: `20140929233650.0Z: 1: ZmwKf34lH1/+NsjIWSfKQSb4H…`
 	*Note*: This communication with Active Directory is encrypted with Kerberos encryption (in rare cases may fallback to NTLM encryption, but is still encrypted)
    **Important**: When management runtime communicates with Active Directory, it avoids talking with RODC and always looks for writable DC to talk with
8. After password and expiration timestamp are successfully reported to AD, the password of managed Administrator account is reset to new value generated in step 5  
    * Reason for this sequence of steps is that we cannot report and reset password as a single transaction.  
    So, we consider the reporting of password to AD as more “error prone” – more things can get wrong as there is network between workstation and domain controller; whereas password reset operation works against local computer.  
    We try to perform the operation considered riskier first to be able to catch any errors prior actually resetting the password.  
    This order of steps minimizes the risk that reported password will be different than actual password of managed Administrator account
9. After successfully resetting the password, CSE finishes execution reporting success to GPO framework that called it
10. In case that some error occurs during the execution, CSE logs the error to Application log and finishes execution, reporting the error to GPO framework that called it</li>

Depending on configured verbosity of logging, management runtime logs activity to Application log of managed machine.  
When configuration requires storage of logs in central location, management runtime also sends relevant events to central logging service.