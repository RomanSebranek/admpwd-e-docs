# Configuration
Management runtime is configurable using registry values specified in the registry key:
`HKLM\Software\Policies\Microsoft Services\AdmPwd`  
Registry values are configured via GPO or DSC, or can be configured by any other means that can keep registry configuration in place.

Currently the following configuration values are supported:  
<table>
<thead>
<tr>
<th width="27%">Value</th>
<th width="17%">Type</th>
<th>Meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td>AdmPwdEnabled</td>
<td>REG_DWORD</td>
<td>Setting to non-zero enables the solution.
Resulting policy must have this value set to non-zero so as the solution is enabled to work on managed machine.  

Managed by policy *Enable local admin password management*</td>
</tr>
<tr>
<td>AdminAccountName</td>
<td>REG_SZ</td>
<td>Name of local account to manage password for.
If not configured, CSE manages built-in Administrator password regardless of its name (detects it via well-known SID)

Managed by policy *Customize administrator account name*/td>
</tr>
<tr>
<td>ManualPasswordChangeProtectionEnabled</td>
<td>REG_DWORD</td>
<td>Setting to zero disables protection against manual changes of managed local administrator account.
If not configured or set to non-zero, protection is active  

Managed by policy *Protect against manual changes of password*</td>
</tr>
<tr>
<td>LogLevel</td>
<td>REG_DWORD</td>
<td>Logging level on clients.

Supported values are specified in [Logging](Logging.md)  
If not configured, default is 0.

Managed by policy *Logging level*</td>
</tr>
<tr>
<td>PasswordLength</td>
<td>REG_DWORD</td>
<td>Length of password generated  

Minimum:    8<br/>
Maximum:    64<br/>
Default:    12<br/>

Managed by policy *Password Settings*</td>
</tr>
<tr>
<td>PasswordComplexity</td>
<td>REG_DWORD</td>
<td>Complexity of generated password  

Minimum: 1  
Maximum: 4  
Default: 4  

Meaning of values:  
1 ... large letters  
2 ... large letters + small letters  
3 ... large letters + small letters + numbers  
4 ... large letters + small letters + numbers + spec chars  

Managed by policy *Password Settings*</td>
</tr>
<tr>
<td>PasswordAge</td>
<td>REG_DWORD</td>
<td>Age of password in hours.  

Minimum: 1  
Maximum: 9999  
Default: 720 (30 days)  

Managed by policy *Password Settings*</td>
</tr>
<tr>
<td>MaxPasswordAge</td>
<td>REG_DWORD</td>
<td>Maximum configurable age of password in hours. When configured, PasswordAge cannot exceed this value.  

Minimum: 1  
Maximum: No limit  
Default: 8760(1 year)  

Managed by policy *Maximum Configurable Password Age*</td>
</tr>
<tr>
<td>PublicKey</td>
<td>REG_SZ</td>
<td>

Base64-encoded public key for password encryption. This is **CryptoAPI** encryption key, used by clients up to 7.5.1.0.
Get the key via PowerShell cmdlet `Get-AdmPwdPublicKey`

Managed by policy *Password encryption*</td>
</tr>
<tr>
<td>EncryptionKey</td>
<td>REG_SZ</td>
<td>

Base64-encoded public key for password encryption. This is **CNG** encryption key, used by clients v7.5.2.0 and newer
Get the key via PowerShell cmdlet `Get-AdmPwdPublicKey`

Managed by policy *Password encryption*</td>
</tr>
<tr>
<td>PwdExpirationProtectionEnabled</td>
<td>REG_DWORD</td>
<td>Whether CSE shall enforce password age to be aligned with PasswordAge parameter.
If set to non-zero, when password expiration time set on computer exceeds PasswordAge policy, password is reset upon next GPO refresh and expiration is set according to policy

Managed by policy *Do not allow password expiration time longer than required by policy*</td>
</tr>
<tr>
<td>PwdEncryptionEnabled</td>
<td>REG_DWORD</td>
<td>Whether or not password encryption is enabled

Default: No  

Managed by policy *Password encryption*</td>
</tr>
<tr>
<td>PwdHistoryEnabled</td>
<td>REG_DWORD</td>
<td>Whether or not to maintain password history for computer  

Managed by policy *Maintain history of passwords*</td>
</tr>
<tr>
<td>BuiltInAdminState</td>
<td>REG_DWORD</td>
<td>Desired state for built-in local admin account on managed machine.  
Meaning of values:  

0  ... Built-in admin account is kept Disabled  
1  ... Built-in admin account is kept Enabled  

Managed by policy *Desired state of built-in admin account*</td>
</tr>
<tr>
<td>ReportingEnabled</td>
<td>REG_DWORD</td>
<td>

Allows centralized reporting of client activity.  
When enabled, managed machines send activity reports to central collecting host.  

Managed by policy *Centralized client activity reporting*</td>
</tr>
<tr>
<td>ReportingHost</td>
<td>REG_SZ</td>
<td>

Fully Qualified Domain Name (FQDN) of PDS host that collects client reports.  
Must be configured when centralized client reporting is enabled (see ReportingEnabled above), and must be reachable over network on UDP transport (for UDP port, see `ReportingPort` below).    

Managed by policy *Centralized client activity reporting*</td>
</tr>
<td>ReportingPort</td>
<td>REG_DWORD</td>
<td>

UDP port where centralized reporting collector listens. By default, it's port 61184. Only needs to be changed/configured, when centralized reporting collector is configured to listen on non-default port.  

Managed by policy *Centralized client activity reporting*</td>
</tr>
</tbody>
</table>  

*Note*: In GPO UI, all configuration settings related to configuration of CSE ale located under `Computer configuration/Administrative Templates/AdmPwd Enterprise/Managed Clients` path.
