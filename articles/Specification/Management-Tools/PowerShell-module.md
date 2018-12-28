# PowerShell module

PowerShell module implements cmdlets as specified in table below:
<table>
<thead>
<tr>
<th>Cmdlet</th>
<th>Description</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr>
<td>Get-AdmPwdADSchema</td>
<td>
Returns information about solution specific attributes in AD schema.  
Useful when finding objectGUID of an attribute</td>
<td>Communicates with Active Directory

No specific permissions needed.</td>
</tr>
<tr>
<td>Update-AdmPwdADSchema</td>
<td>Creates new attributes in AD schema and adds them in may-contain set of computer class

Creates extended rights in configuration partition and makes them applied to computer class</td>
<td>Communicates with Active Directory

Needs Enterprise admin and Schema Admin permission</td>
</tr>
<tr>
<td>Get-AdmPwdPassword</td>
<td>Retrieves password of local admin account for given computer, optionally with password history</td>
<td>Communicates with PDS

Needs Read Admin Password permission

Audited by PDS</td>
</tr>
<tr>
<td>Reset-AdmPwdPasword</td>
<td>Requests reset of password of local admin account for given computer</td>
<td>Communicates with PDS

Needs Reset Admin Password permission

Audited by PDS</td>
</tr>
<tr>
<td>Set-AdmPwdSelfPermission</td>
<td>Delegates permission to manage password of own local admin account to managed computers</td>
<td>Communicates with Active Directory

Needs Write Permissions permission on target container</td>
</tr>
<tr>
<td>Set-AdmPwdPdsPermission</td>
<td>Delegates permission to interact with AD to service account of PDS</td>
<td>Communicates with Active Directory

Needs Write Permissions permission on target container</td>
</tr>
<tr>
<td>Set-AdmPwdPdsDeletedObjectsPermission</td>
<td>Delegates permission to interact with Deleted Objects container in AD to service account of PDS</td>
<td>Communicates with Active Directory

Needs Write Permissions permission on target container</td>
</tr>
<tr>
<td>Set-AdmPwdReadPasswordPermission</td>
<td>Delegates permission to read local admin password</td>
<td>Communicates with Active Directory

Needs Write Permissions permission on target container/object</td>
</tr>
<tr>
<td>Set-AdmPwdResetPasswordPermission</td>
<td>Delegates permission to reset local admin password</td>
<td>Communicates with Active Directory

Needs Write Permissions permission on target container/object</td>
</tr>
<tr>
<td>Update-AdmPwdPasswordHistory</td>
<td>Maintains (trims) password history record for given computer based on given criteria</td>
<td>Communicates with Active Directory

Needs Read/Write + CONTROL_ACCESS permission on computer object</td>
</tr>
<tr>
<td>New-AdmPwdKeyPair</td>
<td>Generates new key pair in PDS</td>
<td>Communicates with PDS

Needs Key Admin role in PDS

Audited in PDS</td>
</tr>
<tr>
<td>Get-AdmPwdPublicKey</td>
<td>Retrieves public part of key pair with given ID from PDS</td>
<td>Communicates with PDS

No special permission needed

Not audited</td>
</tr>
<tr>
<td>Get-AdmPwdPublicKeys</td>
<td>Retrieves public part of all key pairs managed by PDS</td>
<td>Communicates with PDS

No special permission needed

Not audited</td>
</tr>
<tr>
<td>Get-AdmPwdKeySize</td>
<td>Gets list of supported key sizes from PDS</td>
<td>Communicates with PDS

No special permission needed

Not audited</td>
</tr>
<tr>
<td>Get-AdmPwdKeyAdminRoleName</td>
<td>Gets the name of AD group implementing Key Admin role</td>
<td>Communicates with PDS

No special permission needed

Not audited</td>
</tr>
<tr>
<td>Get-AdmPwdUserPermissions</td>
<td>Get information about solution permissions specific user has on specific computer object</td>
<td>Communicates with PDS

No special permission needed

Not audited</td>
</tr>
<tr>
<td>Get-AdmPwdEnvironmentStatus</td>
<td>Retrieves information about the environment</td>
<td>Communicates with PDS

No special permissions needed

Not audited</td>
</tr>
<tr>
<td>Set-AdmPwdPdsManagedAccountsPermission</td>
<td>Sets permissions necessary for PDS to manage password for Managed Domain Accounts</td>
<td>Communicates with Active Directory

Needs Write Permissions permission on target container</td>
</tr>
<tr>
<td>Get-AdmPwdManagedAccountPassword</td>
<td>Retrieves password of Managed Domain Account</td>
<td>Communicates with PDS

Needs Read Admin Password permission

Audited by PDS</td>
</tr>
<tr>
<td>Reset-AdmPwdManagedAccountPassword</td>
<td>Resets password of Managed Domain Account</td>
<td>Communicates with PDS

Needs Read Admin Password permission

Audited by PDS</td>
</tr>
<tr>
<td>Get-AdmPwdUserPermissions</td>
<td>Retrieves list of permissions directly or indirectly granted to user on given AD object (computer or user account)</td>
<td>Communicates with PDS
Not audited</td>
</tr>
</tbody>
</table>

*Note*: For usage of PowerShell commands, refer to cmdlet help that comes with PowerShell module (Get-Help )