# PowerShell module

PowerShell module implements cmdlets as specified in table below:
<table>
<thead>
<tr>
<th>Cmdlet</th>
<th>Description</th>
<th>*Note*</th>
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

Audited in PDS

PDS Admin role required
</td>
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
<td>Gets the name of AD group implementing PDS Admin role.

*Note*: In next release, the cmdlet will be renamed to Get-AdmPwdPdsAdminRoleName
</td>
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
<tr>
<td>Get-AdmPwdPds</td>
<td>Returns list of discovered instances of PDS service along with their parameters</td>
<td>Does not communicate with PDS

Not audited</td>
</tr>
<tr>
<td>Get-AdmPwdPdsSupportedForest</td>
<td>Returns list of supported AD forest as defined in PDS configuration.   
*Note*: PDS Forest is always supported and may not be returned.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Add-AdmPwdPdsSupportedForest</td>
<td>Adds supported AD forest to PDS configuration.   
*Note*: PDS Forest is always supported and does not need to be added.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Set-AdmPwdPdsSupportedForest</td>
<td>Updates configuration of supported AD forest in PDS configuration.   
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Remove-AdmPwdPdsSupportedForest</td>
<td>Removes AD forest from list of supported AD forests defined in PDS configuration.

*Note*: PDS Forest is always supported and cannot be removed.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Get-AdmPwdPdsSidMapping</td>
<td>Returns list of supported SID mappings.

*Note*: SID mappings help define permissions model in topolgies without AD forest trust
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Add-AdmPwdPdsSidMapping</td>
<td>Adds new SID mapping to list of SID mappings defined in PDS configuration.   
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Set-AdmPwdPdsSidMapping</td>
<td>Updates parameters of existing SID mapping defined in PDS configuration.   
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Remove-AdmPwdPdsSidMapping</td>
<td>Removes SID mapping from list of SID mappings defined in PDS configuration.   
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Get-AdmPwdPdsManagedAccountsContainer</td>
<td>Gets list of Managed Accounts Containers as defined in PDS configuration.

*Note*: Managed Accounts Container is AD container where PDS service automatically manages password of user accounts. If you want user account to have password securely managed and its retrieval controlled by PDS, move it to this container.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Add-AdmPwdPdsManagedAccountsContainer</td>
<td>Adds registration of new Managed Accounts Container to list of registered containers defined in PDS configuration.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>

<tr>
<td>Set-AdmPwdPdsManagedAccountsContainer</td>
<td>Updates parameters of registered Managed Accounts Container in PDS configuration.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>
<tr>
<td>Remove-AdmPwdPdsManagedAccountsContainer</td>
<td>Removes registration of Managed Accounts Container from list of registered containers defined in PDS configuration.
</td>
<td>Communicates with PDS

Audited

PDS Admin role required
</td>
</tr>

<tr>
<td>Get-AdmPwdAccessControlParameters</td>
<td>
Returns values of parameters for access control. Acces control is scaled to the following granularity by this attributes: HonorFullControlPermission, HonorAllExtendedRightsPermission, HonorLocalGroupsFromRemoteComputerDomain</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>

<tr>
<td>Get-AdmPwdPdsDnsParameters</td>
<td>
Returns values of DNS SRV record. That is used by service clients to find live instance of the service.</td>
<td>Communicates with !!!potřeba doplnit!!!
No specific permissions needed.</td>
</tr>

<tr>
<td>Get-AdmPwdPdsLicenseParameters</td>
<td>
Returns values of license parameters, specifically these: PdsName and LicenseFilePath</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>

<tr>
<td>Get-AdmPwdPdsManagedAccountsParameters</td>
<td>
Returns values of managed accounts parameters, specifically these: PasswordManagementInterval, DistinguishedName, PasswordAge, KeyId, PasswordComplexity, PasswordLength, PasswordHistory and PasswordHistoryLength.</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>

<tr>
<td>Set-AdmPwdPdsAccessControlParameters</td>
<td>
Sets each access control parameter.</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>

<tr>
<td>Set-AdmPwdPdsDnsParameters</td>
<td>
Sets parameters in DNS SRV record.</td>
<td>Communicates with !!!Nutno doplnit!!!.
No specific permissions needed.</td>
</tr>

<tr>
<td>Set-AdmPwdPdsLicenseParameters</td>
<td>
Sets parameters of license file.</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>

<tr>
<td>Set-AdmPwdPdsManagedAccountsParameters</td>
<td>
Sets each parameter of managed accounts.</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>

<tr>
<td>Move-AdmPwdPdsAdminRole</td>
<td>
Transfer PDS admin rights to new owner.</td>
<td>Communicates with PDS.
No specific permissions needed.</td>
</tr>



</tbody>
</table>

*Note*: For usage of PowerShell commands, refer to cmdlet help that comes with PowerShell module (Get-Help )
