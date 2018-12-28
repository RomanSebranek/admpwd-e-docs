# Configuration

Configuration of PDS service is maintained in `AdmPwd.PDS.exe.config` file. Service recognizes configuration parameters as specified in table below.

**Important**: PDS configuration file gets rewritten during PDS service reinstalls or upgrades. Please make sure you have backup of configuration file for such case.


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
<td>Pds - Dns – Autodiscovery – DomainsToPublish – domain</td>
<td>DNS name of domain where PDS shall publish own SRV record</td>
<td>

When specified, PDS registers SRV record in specified domains only.
PDS own domain must be listed as well so as PDS would register SRV record there.  
If no domain specified, PDS registers SRV record in own domain only.</td>
</tr>
<tr>
<td>Pds - Keystore</td>
<td>Identifier of assembly implementing keystore for key pairs</td>
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

</td>
</tr>
<tr>
<td>Pds – AccessControl - MandatoryGroups</td>
<td>Contains list of SIDs of groups caller has to be member of so as requests for password read and reset was honored. Works as additional protection layer in additions to standard Read/Reset password. Used to enforce Authentication Mechanism Assurance</td>
<td>

*Default*: Empty list, which means that this additional layer of protection is not active</td>
</tr>
<tr>
<td>Pds – PDSAdmin - role</td>
<td>Name of AD group implementing PDS Admin role</td>
<td>

*Default*: Enterprise Admins  

*Note*: PDS Admin role is allowed to perform the following operations:  
* Generate new encryption/decryption key pairs
* Maintain alternate credentials to authorize PDS access to remote forests
</td>
</tr>
<tr>
<td>Pds – License – file</td>
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
<td>Pds – SupportedForests – forest</td>
<td>List of forests managed by PDS. When missing, only local forest where PDS is installed is supported.</td>
<td>

*Default*: Not present, which means that PDS manages only its own AD forest. Forest can contain registration of alternate credentials:  
* **user**: username 
* **password**: password for user specified in username
* **keyId**: ID of key that was used to encrypt the password

*Note*: When alternate creadentials not specified, PDS uses identity of own service account to authenticate access to remote forest.  
*Note*: Local PDS forest is always supported and does not support alternate credentials.
</td>
</tr>
<tr>
<td>KeyStore – path</td>
<td>Path where key pairs are stored</td>
<td>

*Default*: CryptoKeyStorage  
Relative to PDS folder; so, by default, PDS looks for key pairs in `%ProgramFiles%\AdmPwd\PDS\CryptoKeyStorage`
Can be also:
* an absolute path  
* UNC path  
  
</td>
</tr>
<tr>
<td>KeyStore – pathType</td>
<td>Whether path is absolute or relative</td>
<td>

*Default*: Relative

Possible values: Absolute, Relative</td>
</tr>
<tr>
<td>KeyStore – cryptoForNewKeys</td>
<td>Cryptography used to generate new encryption/decryption keys</td>
<td>

Default: CNG

Possible values:
* CNG
* CryptoAPI


*Note*: Support for new keys geberated by CryptoAPI is maintained for compatibilityonly,  and ability to generate new keys using CryptoAPI will be removed in future versions. However, PDS will still be able to decrypt passwords encrypted with CryptoPAPI keys</td>
</tr>
<tr>
<td>KeyStore – SupportedKeySizes – add – value</td>
<td>List of supported key sizes when creating new key pair</td>
<td>PDS only allows to create key pair with one of the specified key sizes</td>
</tr>
<tr>
<td> ManagedAccounts - passwordManagementInterval</td>
<td> How often PDS looks for expired passwords of Managed Domain Accounts</td>
<td> 

*Default*: 600 seconds</td>
</tr>
<tr>
<td>ManagedAccounts - containers - add - distinguishedName</td>
<td>DN of container when PDS looks for Managed Domain Accounts to manage password for</td>
<td></td>
</tr>
<tr>
<td>ManagedAccounts - containers - add - passwordAge</td>
<td>Password age for Managed Domain Accounts in given container, in minutes.</td>
<td>

*Default*: 43200 minutes (30 days)</td>
</tr>
<tr>
<td>ManagedAccounts - containers - add - keyId</td>
<td>ID of encryption key to use to encrypt the password of Managed Domain Account in given container</td>
<td>

*Default*: 0, which means most recent key managed by keystore</td>
</tr>
<tr>
<td>ManagedAccounts - containers - add - passwordComplexity</td>
<td>Required complexity of password for Managed Domain Accounts in given container</td>
<td>

Allowed values:  
1 .. Large letters  
2 .. Large and Small letters  
3 .. Large and Small letters and Numbers  
4 .. Large and Small letters, Numbers and Special characters

*Default*: 4</td>
</tr>
<tr>
<td>ManagedAccounts - containers - add - passwordLength</td>
<td>Required length of password set by PDS on Managed Domain Accounts in given container</td>
<td>

*Default*: 14</td>
</tr>
</tbody>
</table>

Sample of pretty-much default configuration:

```xml
<  <PDS>
    <Dns>
      <Autodiscovery UnregisterOnShutdown="true" RegistrationInterval="86400" Priority="100" Weight="100" TTL="1200">
        <DomainsToPublish>
        </DomainsToPublish>
      </Autodiscovery>
    </Dns>
    <KeyStore assembly="AdmPwd.PDS" typeName="AdmPwd.PDS.KeyStore.FileSystemKeyStore"/>
    <AccessControl HonorFullControlPermission="false">
      <SidMappings>
        <!--<add id="1" primarySid="S-1-5-21-3637775476-1509572741-3331717474-500" mappedSid="S-1-5-21-418575132-3576222476-798353013-1603" description="my account -> target account"/>-->
      </SidMappings>
      <MandatoryGroups>
        <!--<add groupSid="S-1-5-21-3637775476-1509572741-3331717474-512"/>-->
      </MandatoryGroups>
    </AccessControl>
    <PDSAdmin role="Enterprise Admins"/>
    <License file=".\license.xml"/>
    <SupportedForests>
        <!--<add forest="ad.mydomain.com" user="ad.mydomain.com\PDSAlternateAccount" password="GHpyo4fgtres...." keyId="1"/>-->
    </SupportedForests>
  </PDS>
  
  <KeyStore path="CryptoKeyStorage" pathType="Relative" cryptoForNewKeys="CNG">
    <SupportedKeySizes>
      <add value="1024"/>
      <add value="2048"/>
      <add value="3072"/>
      <add value="4096"/>
    </SupportedKeySizes>
  </KeyStore>

  <ManagedAccounts passwordManagementInterval="600">
    <containers>
      <!--<add 
        distinguishedName="OU=MyManagedAccounts,DC=ad,DC=mydomain,DC=com" 
        passwordAge="240" 
        keyId="1" 
        passwordComplexity="LargeSmallNumSpec" 
        passwordLength="12" 
        passwordHistory="false" 
        passwordHistoryLength="3"
      />-->
    </containers>
  </ManagedAccounts>
```
