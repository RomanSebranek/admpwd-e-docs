# Installation
Basic AdmPwd.E installation and configuration consists of the following steps:
* AD schema extension
* Configuration of permissions for PDS service
* Configuration of permissions for users of solution
* Configuration of permissions for computers to report passwords to AD
* Installation of PDS
* Installation of management tools
* Configuration of GPO
* Installation of management runtime

Steps are described in more details below.

## AD schema extension
This step installs required AD Schema updates and registers new AD Extended Rights.

Required role membership:
* Schema Admin (for AD schema updates)
* Enterprise admin (for registration of Extended rights)

Schema can be extended by 2 ways:
* Via PowerShell management module
* By importing of LDF files

### PowerShell management module
This is recommended way of performing AD changes.
To perform AD updates, install PowerShell management module (from `AdmPwd.E.Tools.Setup.<platform>.msi`) on any Windows machine where you can log in as Schema Admin AND Enterprise Admin.

Then run the following PowerShellcommand:

`Update-AdmPwdADSchema`

Output shows details on operations performed - for bot AD schema update AND Extended Rights registration

### LDF files
There are 2 LDF files:
* For [AD Schema changes(https://gcstoragedownload.blob.core.windows.net/download/AdmPwd.E/Latest/AdmPwd_Full.zip)
* For [Extended Rights definition](https://gcstoragedownload.blob.core.windows.net/download/AdmPwd.E/Latest/ExtendedRights.zip)

Files can be imported from command line on Domain Controller by ldifde.exe tool:  

`ldifde -i -f AdmPwd_Full.ldf -c "cn=X" "#schemaNamingContext" -v`  
*Note*: This command requires running as Schema Admin  

`ldifde -i -f ExtendedRights.ldf -c "cn=X" "#configurationNamingContext" -v`  
*Note*: This command requires running as Enterprise Admin

## Configuration of permissions for PDS service

This step ensures that PDS service account has appropriate permissions on AD trees where managed objects are located.

By default, PDS installs under NETWORK SERVICE asccount, but can be also operated under domain account or Group Managed Service Account. Steps in this guide assume that PDS is using NETWORK SERVICE account.

Best practice is to create AD group named "PDS Servers" (or any name confirming to naming convention for your AD) and make computer accounts of servers that are planned to run PDS service a members of this group.  
*Note*: If running PDS on Domain Controller, make NETWORK SERVICE member of this group. This is because when process running under NETWORK SERVICE on Domain Controller accesses AD, it always chooses local AD instance and authenticates as NETWORK SERVICE rather than computer account.

Permissions are granted using `AdmPwd.PS` PowerShell management module. Sample script below assumes that:
* PDS delegation group name is 'PDS Servers'
* Managed machines are in AD subtree under 'ManagedMachines'
* Managed domain accounts are in AD under subtree 'AdmPwdManagedAccounts
* DNS domain name is `MyDomain.com`; NetBIOS domain name is `MYDOMAIN`

```PowerShell
# Permission to read and write ms-MCS-* attributes on computers
Set-AdmPwdPdsPermission -Identity ManagedMachines -AllowedPrincipals "MYDOMAIN\PDS Servers"

# Permission to retrieve passwords from deleted computer objects
# Note: You may need to take ownership of cn=Deleted Objects,DC=MaDomain,DC=com to be able to perform delegation
Set-AdmPwdPdsDeletedObjectsPermission -DomainDnsName mydomain.com

#Permissions to manage password of managed domain accounts
Set-AdmPwdPdsManagedAccountsPermission -Identity AdmPwdManagedAccounts -AllowedPrincipals "MYDOMAIN\PDS Servers"

```

## Configuration of permissions for users of the solution
Solution uses newly registered Extended Rights 'Read admin password' and 'Reset admin password' to allow/deny password reads and resets. This provides ability to define security model that does not depend on built-in AD permissions.

Sample delegation script below has the following assumptions:
* Managed machines are in AD subtree under 'ManagedMachines'
* Managed domain accounts are in AD under subtree 'AdmPwdManagedAccounts
* DNS domain name is `MyDomain.com`; NetBIOS domain name is `MYDOMAIN`
* There is AD group that allows password reads: 'Password Readers'
* There is AD group that allows password resets: 'Password resetters'

```PowerShell
#Delegate Read password permission for managed machines
Set-AdmPwdReadPasswordPermission -Identity ManagedMachines -AllowedPrincipals "MYDOMAIN\Password Readers"

#Delegate Reset password permission for managed machines
Set-AdmPwdResetPasswordPermission -Identity ManagedMachines -AllowedPrincipals "MYDOMAIN\Password Resetters"
```

Permissions can also be delegated on individual object level. In this case, you have to use distinguishedName of object in delegation command. This is particularly useful when you want to delegate permission to use privileged personal account to standard account:

```PowerShell
#Allow mydomain\john to read password of account mydomain\john_admin
Set-AdmPwdReadPasswordPermission -Identity "cn=john_admin,ou=AdmPwdManagedAccounts,dc=mydomain,dc=com" -AllowedPrincipals "MYDOMAIN\john"
```

## Permissions for managed computers to report passwords to AD
This makes sure that managed permissions can write passwords of managed local accounts to attributes of own computer accounts.

```PowerShell
#Allow mydomain\john to read password of account mydomain\john_admin
Set-AdmPwdComputerSelfPermission -Identity ManagedMachines
```

## Installation of PDS
PDS gets installed from `AdmPwd.E.Tools.Setup.<platform>.msi`. Simply run the msi package manually and select appropriate component to install. PDS installs all files to `%ProgramFiles%\AdmPwd\PDS`
Or - if you prefer unattended setup - this is command line that installs just PDS service on x64 machine:  
`Msiexec /i AdmPwd.E.Tools.Setup.x64.msi ADDLOCAL=PDS /q`

## Installation of management tools
Management tools that come with the solution include Fat UI client and Powershell management module.

Fat UI client is best option for task-oriented staff and allows easily read and reset password of local admin accounts on managed computers.

Powershell module implements all operations provided by solution, and (almost) complete configuration of solution.  
*Note*: Some configuration settings are performed via editing of PDS configuration file

Management tools get installed from `AdmPwd.E.Tools.Setup.<platform>.msi`. Simply run the msi package manually and select appropriate component to install.  
Fat client installs all files to `%Program Files%\AdmPwd\UI`; PowerShell module gets installed tomodules  standard path `$pshome\Modules\AdmPwd.PS`

If you prefer unattended installation, this is a command line that installs Fat client UI and PowerShell module on x64 machine:

`Msiexec /i AdmPwd.E.Tools.Setup.x64.msi ADDLOCAL=Management.PS,Management.UI /q`

## Configuration of GPO
Configuration of GPO is done using solution specific ADMX templates. Templates are installed from `AdmPwd.E.Tools.Setup.<platform>.msi` to standard folder `%SystemRoot%\PolicyDefinitions`
Install them on machine where you perform GPO management. If you use Centralized POlicy Store in your organization, copy the respective files to folder that implements your Central policy store. Files are:  
```
AdmPwd.E.admx
en-US\AdmPwd.E.adml
```

*Note*: On Windows Nano Server, GPO is not there, any you have to use PowerShell DSC or other trechnology to implement registry configuration. There is sample PowerShell DSC configuration in Windows Nano Server install package that shows possible configuration setings.

## Installation of management runtime
Installation of management runtime is dome using `AdmPwd.E.CSE.Setup.<platform>.msi` on all Windows platforms except for Windows Nano Server. On Windows Nano server, management runtime is installed from `AdmPwd.E.Client.appx` package.

Deploy installers or packages via SW distribution tool of your choice. Unatended setup command line for MSI package:  
`msiexec /i AdmPwd.E.CSE.Setup.<platform>.msi`

*Note*: Remember to set policy 'Enable local admin password management' to Enabled once you are ready and management runtime is installed. Without this policy turned on, management runtime does not start machine management.
