# Logging
Management runtime logs all events in Application Event Log of local computer. Log messages are English only, but can be localized or additional language can be added, if necessary, in the future.

Type of events that are logged is configurable either via GPO or via the following registry `REG_DWORD` value:  
`HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\GPExtensions\{D76B9641-3288-4f75-942D-087DE603E3EA}\ExtensionDebugLevel`

*Note*: This registry value takes precedence over Logging level registry policy - see [Configuration](Configuration.md) section for details

Semantic of possible values is as follows:
<table>
<thead>
<tr>
<th width="11%">Value</th>
<th>Meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td>0</td>
<td>Silent mode; log errors only.  

When no error occurs, no information is logged about management runtime activity  

This is a default value</td>
</tr>
<tr>
<td>1</td>
<td>Log Errors and warnings</td>
</tr>
<tr>
<td>2</td>
<td>Verbose mode, log everything</td>
</tr>
</tbody>
</table>

Event source for all events reported by management runtime is always **AdmPwd**.  
The following table summarizes the events that can occur in the Event Log:
<table>
<thead>
<tr>
<th width="5%">ID</th>
<th width="15%">Severity</th>
<th width="30%">Description</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr>
<td>2</td>
<td>Error</td>
<td>Could not get computer object from AD. Error %1</td>
<td>This event is logged in case that management runtime is not able to connect to computer account for local computer in AD.

%1 is a placeholder for error code returned by function that retrieves local computer name, converts it to DN and connects to AD object specified by the DN</td>
</tr>
<tr>
<td>3</td>
<td>Error</td>
<td>Could not get local Administrator account. Error %1</td>
<td>This event is logged in case that management runtime is not able to connect to built-in Administrator account.

%1 is a placeholder to error code returned by function that detects the name of local administrator’s account and connects to the account</td>
</tr>
<tr>
<td>4</td>
<td>Error</td>
<td>Could not get password expiration timestamp from computer account in AD. Error %1.</td>
<td>This event is logged in case that management runtime is not able to read the value of `ms-MCS-AdmPwdExpirationTime` of computer account in AD.  

%1 is a placeholder for error code returned by function that reads the value of the attribute and converts the value to <code>unsigned __int64</code> type</td>
</tr>
<tr>
<td>6</td>
<td>Error</td>
<td>Could not create new password. Error %1.</td>
<td>This event is logged when management runtime for any reason (typically because of failure to initialize/use random number generator) cannot create new password for local admin account</td>
</tr>
<tr>
<td>7</td>
<td>Error</td>
<td>Could not encrypt password. Error %1.</td>
<td>This event is logged in any of the following situations:  

* Management runtime cannot locate public key in registry  
* Public key blob stored in GPO is invalid  
* RSA CSP is not able to encrypt the password  

%1 is a placeholder for error returned by CryptoAPI or CNG
</td>
</tr>
<tr>
<td>8</td>
<td>Error</td>
<td>Could not write changed password to AD. Error %1.</td>
<td>This event is logged in case that management runtime is not able to report new password and timestamp to AD.  

%1 is a placeholder for error code returned by LDAP modify request</td>
</tr>
<tr>
<td>9</td>
<td>Error</td>
<td>Could not reset local Administrator's password. Error %1</td>
<td>This event is logged in case that management runtime is not able to reset the password of built-in Administrator account.    

%1 is a placeholder for error returned by `NetUserSetInfo()` API call</td>
</tr>
<tr>
<td>12</td>
<td>Error</td>
<td>Could not check if password is in sync with AD. Error %1.</td>
<td>This error is logged when management runtime is not able to detect password age of managed local administrator account.  

%1 is placeholder for error returned by `NetUserGetInfo()` API call</td>
</tr>
<tr>
<td>13</td>
<td>Error</td>
<td>Could not check or set state of built-in admin account. Error %1.</td>
<td>This error is logged when management runtime is not able to detect state of built-in local administrator account.  

%1 is placeholder for error returned by `NetUserGetInfo()` API call</td>
</tr>
<tr>
<td>100</td>
<td>Information</td>
<td>Beginning processing with flags %1.</td>
<td>
This event is logged when management runtime starts management cycle.  

%1 is placeholder for value of flag passed to `ProcessGroupPolicy()` entry point by GPO framework  
*Note*: On Nano server, this event does not contain information about GPO flag value as there is not GPO.
</td>
</tr>
<tr>
<td>101</td>
<td>Information</td>
<td>It is not necessary to change password yet. Will be changed in %1 days, %2 hours.</td>
<td>This event is logged in case that management runtime detects that it is not yet the time to reset the password of managed admin account</td>
</tr>
<tr>
<td>103</td>
<td>Information</td>
<td>Local Administrator's password has been successfully encrypted</td>
<td>This event is logged when password is successfully encrypted</td>
</tr>
<tr>
<td>104</td>
<td>Information</td>
<td>Local Administrator's password has been reported to AD.</td>
<td>This event is logged when password is successfully reported to AD</td>
</tr>
<tr>
<td>105</td>
<td>Information</td>
<td>Local Administrator's password has been changed</td>
<td>This event is logged after management runtime resets the password of managed admin account</td>
</tr>
<tr>
<td>106</td>
<td>Information</td>
<td>Admin password was not manipulated with (%1)</td>
<td>This event is logged when management runtime detects that password of managed local administrator account was not manipulated with.
%1 is placeholder for difference between expected and real password age, in seconds. Accepted difference is up to 3 seconds</td>
</tr>
<tr>
<td>107</td>
<td>Information</td>
<td>Admin password was never managed on this machine. Resetting password now.
</td>
<td>This event is logged when management runtime detects that password of managed local administrator account was never managed.
</td>
</tr>
<tr>
<td>110</td>
<td>Information</td>
<td>Finished successfully</td>
<td>This event is logged after management runtime performed all required tasks and is about to finish.</td>
</tr>
<tr>
<td>200</td>
<td>Warning</td>
<td>Password expiration too long for computer (%1 days, %2 hours). Resetting password now.</td>
<td>This event is logged in case that management runtime detects that password expiration for computer is longer than allowed by policy in place while protection against excessive password age is turned on</td>
</tr>
<tr>
<td>201</td>
<td>Warning</td>
<td>Password was manipulated with since last check (%1 seconds after regular password change). Resetting password now.</td>
<td>This event is logged when management runtime detect that password of managed local administrator account was changed outside of solution (such as manually by user with administrative permission).</td>
</tr>
<tr>
<td>202</td>
<td>Warning</td>
<td>Admin account management not enabled, exiting</td>
<td>This event is logged when admin account management is not enabled and management runtime is not allowed to work.</td>
</tr>
<tr>
<td>203</td>
<td>Warning</td>
<td>State of built-in admin account differs from policy and was fixed</td>
<td>This event is logged after management runtime detects that state of built-in admin account on managed machine is different than required by the policy and management runtime changed it to be the same as required.</td>
</tr>
</tbody>
</table>

*Notes*:
* Generally, all events with severity “Error” are blocking, so in case that any error occurs, no other tasks are performed and management runtime terminates its processing
* Event source for the Event Log is embedded in the same executable as main GPO executive. Reason for this decision was to make the deployment simple