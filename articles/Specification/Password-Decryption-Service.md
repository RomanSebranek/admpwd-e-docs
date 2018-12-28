# Password Decryption Service
Password Decryption Service (PDS) plays the following key roles:  
* Handling password retrieval and password reset requests from users
* Management of passwords for Managed Domain Accounts
 * Maintenance of key pairs for password encryption/decryption when stored in AD
* Enforcement of auditing
* License validation
* Central reporting of client activity


PDS is designed to operate in multiple AD domains and forests. This means that AD domain / forest can be covered by instance of PDS running in different AD domain / forest, provided that the following prerequisites are met:
* There is at least one-way trust relationship between forest where PDS is installed (PDS forest) and other forest, so as permissions can be granted to security principals from PDS forest in the other forest
  * Or alternate credentials are stored in PDS configuration to use for authentication in remote forest.  
  	In such case, trust relationship is not required to be in place, as long as alternate credentials are resolvable by remote forest
* Permissions to read password, reset password and for PDS itself are granted using objects resolvable from PDS domain / forest, such as global groups from PDS forest  
  * Or - in case of untrusted forest connected via alternate credentials - SID mapping is configured
* PDS is allowed to perform authenticated LDAP operations against domain controllers of the other domain / forest
* Any AD domain where users of PDS service have user accounts, has SRV records for PDS created in its DNS zone pointing to instance of PDS server(s), so as client tools can locate PDS instance
  * Or GPO "*PDS to be used*" is configured and applies to machines running administrative tools

## Management of password of domain user accounts
One of tasks PDS is designed for is secure management of password of domain accounts. This feature is extremely useful for management of secondary accounts that are used for administration of applications and are not routinely used for logon to workstation. PDS helps to keep password secure by the following:
* Regularly changes password of managed account(s) to random value, and stores password encrypted with managed account (in AD attribute `ms-MCS-AdmPwd`)
* Allows to set access control so only eligible people have permission to read the password
* PDS provides password for managed domain account on demand, to eligible persons
* at the same time, PDS records audit trail, so it's always known who and when read the password of managed domain account

This means users of such secondary accounts do not need to remember their passwords, take care for password changes, etc. This significantly increases security of password of passwords because password is just kept securely in AD and available when needed, and users are not tempted to store it in their personal vaults, in their devices that leave organization with them.

AdmPwd.E Integration SDK makes it easy to incorporate automatic password retrieval for managed domain accounts into other tools, like RDP managers and RunAs tools - so eligible users even do not need to know the password when they need to use it, and do not need to deal with complexity of password that is typically required for accounts of this type.

## Logging of activity
PDS logs all activities into dedicated Windows log: Application and Service Logs - GreyCorbel-AdmPwd.E-PDS/Operational.  
Specification of logging for PDS is provided in [dedicated section](Password-Decryption-Service/Logging.md)

## Implementation
PDS is implemented as Win32 service with name “AdmPwd.E PDS” and display name “*AdmPwd.E Password Decryption Service*”.  
Service executable and all related files are installed to %ProgramFiles%\AdmPwd\PDS, and is installed to run under NETWORK SERVICE account by default.

Configuration of service is managed by config file AdmPwd.PDS.exe.config, located in service install folder.

## Keystore for encryption/decryption keys
By default, PDS keeps decryption keys as separate files (1 file per key pair) in CryptoKeyStorage subfolder.  
Setup program sets ACL on this subfolder so as only SYSTEM, Administrators and NETWORK SERVICE have access to it.  
File system based key store is default option available in PDS setup. File system supports local and file share key storage - default is local storage.  
PDS also supports extensibility to allow implementation of custom key storage. Currently available key storage options are:
* **File system keystore**: stores keys on file system (by default in CryptoKeyStorage folder in PDS install folder). This keystore isalways  installed with PDS.
* **Azure KeyVault keystore**: Stores keys in Azure KeyVault as secrets. Loads the keys from KeyVault upon PDS service startup.  
  *Note*: Implementation of this keystore is available as open source solution on <a href="https://github.com/greycorbel/admpwd-e/tree/master/KeyStores/AzureKeyVaultStore" target="_blank" rel="noopener noreferrer">GitHub</a>
* **HSM keystore**: This type of keystore is available with any HSM supported by <a href="https://www.cryptomathic.com/products/key-management/crypto-service-gateway" target="_blank" rel="noopener noreferrer">Cryptomathic Crypto Service Gateway (CSG)</a>  
  *Note*: This type of keystore is subject of separate licensing

## PDS discovery
Management tools do not need to be specifically configured to be able to reach instance of PDS. PDS is automatically discovered by logic implemented in Integration SDK that comes with solution and is used by all client tools that come with the solution. Integration SDK supports the following ways to discover instance of PDS:  
* **DNS SRV records**: PDS service registers and maintains DNS SRV record that is used by service clients to find live  instance of the service.  
* **Registry policy** `PDSList`: Registry value that can contain  or more names (and tcp ports) that management clients shall use

*Note*: When both mechanisms are in place, registry value takes pecedence over SRV records
*Note*: More on PDS autodiscovery in section [PDS Discovery](Password-Decryption-Service/Service-Discovery.md)

## Logging
PDS setup registers file PDS.Messages.dll as event message and resource file for Event Viewer. PDS logs data to 2 channels:
* **Operations channel**: All activity, licensing and auditing information
* **Client activity reports**: Events forwarded by managed machines

*Note*: More on PDS logging in sections [PDS logging](Password-Decryption-Service/Logging.md) and [PDS Centralized reporting](Password-Decryption-Service/Centralized-Client-Logging.md)

