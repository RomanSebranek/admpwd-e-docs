# Configuration

Configuration of PDS service is maintained in `PDS.config` file. Service recognizes configuration parameters as specified in table below.

*Note*: This file is created upon first start of PDS service with default values. Changes then can be made either manually, or via PowerShell cmdlets. File content is preserved during uninstalls and upgrades of PDS service.

PowerShell cmdlets that modify content of PDS.config file are:

Supported Forest management:
- Add-AdmPwdPdsSupportedForest
- Set-AdmPwdPdsSupportedForest
- Remove-AdmPwdPdsSupportedForest

Managed Accounts Containers:
- Add-AdmPwdPdsManagedAccountsContainer
- Set-AdmPwdPdsManagedAccountsContainer
- Remove-AdmPwdPdsManagedAccountsContainer

SID Mappings:
- Add-AdmPwdPdsSidMapping
- Set-AdmPwdPdsSidMapping
- Remove-AdmPwdPdsSidMapping

It's strongly recommended to use PowerShell cmdlets above to modify configuration of Supported forests, Managed Accounts Containers and SID mappings.

*Note*: Future releases will bring more PowerShell cmdlets for management of PDS configuration.

Table below specifies PDS service configurable parameters.

<table>
<thead>
<tr>
<th>Parameter</th>
<th>Meaning</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr>
<td>Pds - Dns – Autodiscovery - RegistrationInterval</td>
<td>Interval for DNS SRV record refresh, in seconds.
PDS automatically refreshes its own SRV record to prevent expiration</td>
<td>

*Default*: 86400 (1 day)  
Setting to `0` disables SRV record registration and refresh. This is useful in environments where PDS service account does not have permission to write to DNS.
</td>
</tr>
<tr>
<td>Pds - Dns – Autodiscovery - UnregisterOnShutdown</td>
<td>Whether PDS shall unregisters its own DNS SRV record during service shutdown</td>
<td>

*Default*: False</td>
</tr>
<tr>
<td>Pds - Dns – Autodiscovery - Priority</td>
<td>Priority of SRV record being created by instance of PDS</td>
<td>

*Default*: 100</td>
</tr>
<tr>
<td>Pds - Dns – Autodiscovery - Weight</td>
<td>Weight of SRV record being created by instance of PDS</td>
<td>

*Default*: 100  
*Note*: PDS discovery logic implemented in Integration SDK ignores weight of SRV records – thus does not perform load balancing
</td>
</tr>
<tr>
<td>Pds - Dns – Autodiscovery - TTL</td>
<td>TTL of registered SRV record, in seconds</td>
<td>

*Default*: 1200 (20 minutes)</td>
</tr>
<tr>
<td>Pds - Dns – Autodiscovery – DomainsToPublish – Domain - DnsName</td>
<td>DNS name of domain where PDS shall publish own SRV record</td>
<td>

*Default*: Empty list which means that PDS registers SRV recordin ow domain only.

When specified, PDS registers SRV record in specified domains only.
PDS own domain must be listed as well so as PDS would register SRV record there.  
If no domain specified, PDS registers SRV record in own domain only.</td>
</tr>
<tr>
<td>Pds - Keystore</td>
<td>Identifier of assembly implementing keystore for key pairs.

Do not change parameters here unless you know what are you doing.
</td>
<td>

PDS supports extensibility and different implementations of keystore.

*Note*: Default keystore that comes with the solution is of type `AdmPwd.PDS.KeyStore.FileSystemKeyStore` and is implemented in main PDS executable.
</td>
</tr>
<tr>
<td>Pds – AccessControl - HonorFullControlPermission</td>
<td>

Specifies whether or not to honor Full Control permission on computer/user object when performing authorization checks for password reads and resets.

When set to TRUE, users who have Full control permission on computer objects can read and reset local admin password even when they are not given explicit permissions as specified in [Extended Rights specification](../Active-Directory/Extended-Rights.md)
</td>
<td>

*Default*: False<br/>
(Full control right on AD object does NOT give permission to read/reset admin password)</td>
</tr>
<tr>
<td>Pds – AccessControl - SidMappings</td>
<td>Maps primary SID (from PDS forest) to SID from untrusted forest managed by PDS. Used to support access control when accessing untrusted AD forest</td>
<td>

*Default*: Empty list

Use PowerShell to manage configuration of SID mappings

</td>
</tr>
<tr>
<td>Pds – AccessControl - MandatoryGroups - Group - Sid</td>
<td>Contains list of SIDs of groups caller has to be member of so as requests for password read and reset was honored. Works as additional protection layer in additions to standard Read/Reset password. Used to enforce Authentication Mechanism Assurance</td>
<td>

*Default*: Empty list, which means that this additional layer of protection is not active</td>
</tr>
<tr>
<td>Pds – PDSAdmin - Role</td>
<td>Name of AD group implementing PDS Admin role</td>
<td>

*Default*: Enterprise Admins  

*Note*: PDS Admin role is allowed to perform the following operations:  
* Generate new encryption/decryption key pairs
* Maintain alternate credentials to authorize PDS access to remote forests
* Maintain list of Supported Forests, Managed Accounts Containers and SID mappings
</td>
</tr>
<tr>
<td>Pds – License – File</td>
<td>Path to license file that unlocks the solution from freeware mode</td>
<td>

*Default*: license.xml

Relative to PDS folder; so, by default, PDS looks for license.xml file in %ProgramFiles%\AdmPwd\PDS

Can be also:
* an absolute path
* UNC path
</td>
</tr>
<tr>
<td>Pds – SupportedForests – Forest - DnsName</td>
<td>List of forests managed by PDS. When missing, only local forest where PDS is installed is supported.
</td>
<td>

*Default*: Not present, which means that PDS manages only its own AD forest.

Forest can contain registration of connection credentials:  
* **User**: username 
* **Password**: password for user specified in username; encrypted by PDS encryption key
* **KeyId**: ID of key that was used to encrypt the password

*Note*: When alternate creadentials not specified, PDS uses identity of own service account to authenticate access to remote forest.  
*Note*: Local PDS forest is always supported and does not support alternate credentials.
</td>
</tr>
<tr>
<td>PDS - FileSystemKeyStore – Path</td>
<td>Path where keystore stores key pairs</td>
<td>

*Default*: CryptoKeyStorage  
Relative to PDS folder; so, by default, PDS looks for key pairs in `%ProgramFiles%\AdmPwd\PDS\CryptoKeyStorage`
Can be also:
* an absolute path  
* UNC path  
  
</td>
</tr>
<tr>
<td>PDS - FileSystemKeyStore – PathType</td>
<td>Whether path is absolute or relative</td>
<td>

*Default*: Relative

Possible values: Absolute, Relative</td>
</tr>
<tr>
<td>PDS - FileSystemKeyStore – CryptoForNewKeys</td>
<td>Cryptography used to generate new encryption/decryption keys</td>
<td>

Default: CNG

Possible values:
* CNG
* CryptoAPI


*Note*: Support for new keys generated by CryptoAPI is maintained for compatibility only,  and ability to generate new keys using CryptoAPI will be removed in future versions. However, PDS will still be able to decrypt passwords encrypted with CryptoPAPI keys</td>
</tr>
<td>PDS - FileSystemKeyStore – FavorOAEP</td>
<td>Whether to try try OAEP padding first or PKCS padding first on decryption.

*Note*: This setting is for compatibility only and will be removed in future versions.
</td>
<td>

Default: true
</td>
</tr>

<tr>
<td>PDS - FileSystemKeyStore – SupportedKeySizes – add – KeySize</td>
<td>List of supported key sizes when creating new key pair</td>
<td>PDS only allows to create key pair with one of the specified key sizes</td>
</tr>
<tr>
<td>PDS - ManagedAccounts - PasswordManagementInterval</td>
<td> How often PDS looks for expired passwords of Managed Domain Accounts in registered Managed Accounts Containers</td>
<td> 

*Default*: 600 seconds</td>
</tr>
<tr>
<td>PDS - ManagedAccounts - Containers - Add - DistinguishedName</td>
<td>DN of container when PDS looks for Managed Domain Accounts to manage password for</td>
<td>

*Default*: Empty list

Use PowerShell to manage configuration of Managed Accounts Containers
</td>
</tr>
<tr>
<td>PDS - ManagedAccounts - Containers - Add - PasswordAge</td>
<td>Password age for Managed Domain Accounts in given container, in minutes.</td>
<td>

*Default*: 43200 minutes (30 days)</td>
</tr>
<tr>
<td>PDS - ManagedAccounts - Containers - Add - KeyId</td>
<td>ID of encryption key to use to encrypt the password of Managed Domain Account in given container</td>
<td>

*Default*: 0, which means most recent key managed by keystore</td>
</tr>
<tr>
<td>PDS - ManagedAccounts - Containers - Add - PasswordComplexity</td>
<td>Required complexity of password for Managed Domain Accounts in given container</td>
<td>

Allowed values:  
Large .. Large letters  
LargeSmall .. Large and Small letters  
LargeSmallNum .. Large and Small letters and Numbers  
LargeSmallNumSpec .. Large and Small letters, Numbers and Special characters

*Default*: LargeSmallNumSpec</td>
</tr>
<tr>
<td>ManagedAccounts - containers - add - passwordLength</td>
<td>Required length of password set by PDS on Managed Domain Accounts in given container</td>
<td>

*Default*: 12</td>
</tr>
</tbody>
</table>

Sample of configuration file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<PDS xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <SupportedForests>
    <Forest DnsName="myRemoteForest.com" />
  </SupportedForests>
  <Dns>
    <Autodiscovery UnregisterOnShutdown="true" RegistrationInterval="86400" Priority="100" Weight="100" TTL="1200">
      <DomainsToPublish>
        <Domain DnsName="myRemoteForest.com" />
      </DomainsToPublish>
    </Autodiscovery>
  </Dns>
  <KeyStoreType Assembly="AdmPwd.PDS" TypeName="AdmPwd.PDS.KeyStore.FileSystemKeyStore" />
  <AccessControl HonorFullControlPermission="false" HonorAllExtendedRightsPermission="false" HonorLocalGroupsFromRemoteComputerDomain="false">
    <SidMappings>
    </SidMappings>
    <MandatoryGroups />
  </AccessControl>
  <PDSAdmin Role="Enterprise Admins" />
  <License File=".\license.xml" />
  <FileSystemKeyStore Path="CryptoKeyStorage" PathType="Relative" CryptoForNewKeys="CNG" FavorOAEP="true">
    <SupportedKeySizes>
      <add KeySize="2048" />
      <add KeySize="3072" />
      <add KeySize="4096" />
    </SupportedKeySizes>
  </FileSystemKeyStore>
  <ManagedAccounts PasswordManagementInterval="600">
    <Containers>
      <add DistinguishedName="OU=Managed Domain Accounts,DC=mydomain,DC=com" PasswordAge="43200" KeyId="1" PasswordComplexity="LargeSmallNumSpec" PasswordLength="13" PasswordHistory="true" PasswordHistoryLength="3" />
      <add DistinguishedName="OU=Managed Domain Accounts,DC=myRemoteDomain,DC=com" PasswordAge="43200" KeyId="1" PasswordComplexity="LargeSmallNum" PasswordLength="16" PasswordHistory="false" PasswordHistoryLength="0" />
    </Containers>
  </ManagedAccounts>
</PDS>
```
