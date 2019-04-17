# Upgrade from Microsoft LAPS
## Introduction
AdmPwd.E is designed for easy coexistence with and upgrade from Microsoft LAPS. Key facts:  
* LAPS installer and AdmPwd.E CSE installer share the same product code, and AdmPwd.E installer is higher version. This means that AdmPwd.E installer automatically upgrades installation of Microsoft LAPS
  * *Note*: LAPS has single installer for all components, including Powershell module and Fat client UI. When upgraded to AdmPwd.E CSE, those components are uninstalled - this is because AdmPwd.E has separate installer for management tools that contains Powershell module and Fat client UI.  
  This means that upgrade is specifically designed for LAPS installation on managed machines that have installed only LAPS CSE component.
* Both LAPS and AdmPwd.E CSE writes passwords and other data to computer account in Active Directory. AdmPwd.E fully recognizes format of data written to Active Directory by LAPS CSE. <br/>This means that AdmPwd.E tools can read/reset passwords for computers managed by LAPS CSE
* LAPS management tools (Powershell and Fat client) work directly with Active Directory to read/reset passwords, while AdmPwd.E management tools always talk to PDS service for password reads/resets. AdmPwd.E also uses different permission model and does not rely on built-in AD permissions.<br/>This means that:
  * AdmPwd.E requires permission model to be configured again on the managed machines
  * Permission mmodel for LAPS and AdmPwd.E can co-exist together side-by-side

## Upgrade steps
To upgrade from Microsoft LAPS to AdmPwd.E, follow the steps in order as specified below.
### Extend AD schema
AdmPwd.E AD schema is superset of LAPS AD schema. Also, AdmPwd.E defines solution-specific AD extended rights that are used by PDS service for access control decisions.  
It is required to run command `Update-AdmPwdADSchema` from AdmPwd.E Powershell module to extend AD schema and define new extended rights. to do so perform the following:
* Install AdmPwd.E Powershell module on any domain joined machine where you're able to log on as member of Schema admins and Enterprise admins groups
* Run Powershell and import AdmPwd.E module via `Import-Module AdmPwd.PS`
* Run command `Update-AdmPwdADSchema`

Expected output of command is similar to the example below:
```
PS C:\windows\system32> Update-AdmPwdADSchema

DistinguishedName                                Operation                 Status
-----------------                                ---------                 ------
cn=ms-MCS-AdmPwdExpirationTime,CN=Schema,CN=...  AddSchemaAttribute        EntryAlreadyExists
cn=ms-MCS-AdmPwd,CN=Schema,CN=Configuration...   AddSchemaAttribute        EntryAlreadyExists
cn=ms-MCS-AdmPwdHistory,CN=Schema,CN=Conf...     AddSchemaAttribute        Success
cn=user,CN=Schema,CN=Configuration,DC=...        ModifySchemaClass         Success
CN=ms-Mcs-AdmPwdReadPassword,CN=Extended-Righ... AddExtendedPermission     Success
CN=ms-Mcs-AdmPwdResetPassword,CN=Extended-Rig... AddExtendedPermission     Success

```
### Configure permission model
This step defines permision model for PDS service and for users of the solution. Use the same Powershell session as for AD schema extension above and simply follow the steps defined in Installation guide: 
* [Permissions for PDS service](Install.md#configuration-of-permissions-for-pds-service) - this steps define necessary permisisons to PDS service identity to talk to AD.
* [Permission for computers to report password to AD](Install.md#permissions-for-managed-computers-to-report-passwords-to-ad) - this step extends existing permissions for managed computers to report password to AD (adds permission to write password history)
* [Permissions for users](Install.md#configuration-of-permissions-for-users-of-the-solution) - this step configures AdmPwd.E specific permissions for reading and resetting password, that are recognized by PDS service

### Install PDS service
This simple step covers the installation of PDS service that is used as trusted middleware that processes requests of users. Unlike Microsoft LAPS where user tools talk to Active Directory, in AdmPwd.E user tools talk to PDS service. PDS service then uses security context of its service account to talk to Active Directory. This architecture enables easier permission and auditing model of AdmPwd.E.
This step is described in Install guide in [Installation of PDS](Install.md#installation-of-pds) chapter.

### Install management tools
In this step, you're replacing LAPS client tools by AdmPwd.E client tools. AdmPwd.E management tools do not provide upgrade capability for LAPS client tools (this is because LAPS installer is all in one package and upgrade for it is provided by AdmPwd.E installer, as described [earlier](#introduction))  
Simply uninstall LAPS management tools and install AdmPwd.E management tools instead. Like LAPS Fat client UI, AdmPwd.E client UI can be run from netwoerk share and can be registered as ADUC context menu extension.

### Configure GPO
AdmPwd.E provides GPO admin templates that allow to configure and distribute solution configuration values.
LAPS and AdmPwd.E configuration is fully compatible (LAPS GPO is subset of AdmPwd.E GPO) with the following exception: LAPS defines Password Age parameter value in days, while AdmPwd.E defines Password Age in hours. This means that while AdmPwd.E continues using other GPO parameters already defined for LAPS, you are required to configure Password Age GPO again for AdmPwd.E.

### Distribute client management runtime
At this moment, you have configured all necessary settings on management infrastructure side. Users of solution still can use the solution - AdmPwd.E management tools fully support data stored in AD by LAPS management runtime and allow reading and resetting of passwords managed by LAPS. However, to use new functionalities brought by AdmPwd.E, you are required to upgrade management runtime on managed clients.
The process is very simple: just run AdmPwd.E CSE installer on managed manchines and it will automatically upgrade existing LAPS CSE. AdmPwd.E supports unattended installation; for details, see [Installation of management runtime](Install.md#installation-of-management-runtime) in Install guide

## Cleanup
After you have installed AdmPwd.E on all managed clients and all users use AdmPwd.E management tools, it's time to perform cleanup of LAPS artefacts that are no longer used. Cleanup includes:
* [Cleanup of AD schema](#cleanup-of-ad-schema)
* [Cleanup of AD permissions](#cleanup-of-ad-permissions)
* [Cleanup of GPO](#cleanup-of-gpo)

### Cleanup of AD schema
Cleanup of AD schema includes single task: Removal of ms-Mcs-AdmPwd* attributes from may-Contain atribute of computer class in AD schema. This is no longer needed, because AdmPwd.E adds all necessary attributes to user class in AD schema instead, and user class is superclass of computer class - computer class inherits this setting.  
Simply use any LDAP editor, such as AdsiEdit, to perform this task.

### Cleanup of AD permissions
AdmPwd.E contains own permissions model that is based on solution specific AD extended rights. Permissions model used by LAPS relies on native AD permissions and is no longer needed by AdmPwd.E. Use standard AD management tools, such as ADUC, to remove standard AD permissions granted to users of the solution.
We are publishing LAPS Permissions audit and removal script on [GitHub](https://github.com/GreyCorbel/admpwd-e/blob/master/LAPSCleanup/LAPSCleanup.ps1) - check it out!

**Important** Be sure not to remove AdmPwd.E permissions ('Read admin password' and 'Reset admin password') - those permissions are required by AdmPwd.E.

### Cleanup of GPO
Cleanup of GPO is simple task that makes sure that unused GPO policies do not remain in existing GPOs. As mentioned above, AdmPwd.E GPO policies are superset of LAPS GPO policies with one exception: Password Age. Thus, after configuration of GPO for AdmPwd.E, GPO contains 2 policies for Password Age: 1 policy for LAPS, and 1 policy for AdmPwd.E. LAPS policy can be removed anfter upgrade to AdmPwd.E.  
To remove LAPS policy, simply use machine that uses LAPS ADMX templates, edit respective GPOs and set policy 'Password Settings' (under 'AdmPwd' folder) as 'Not configured'.
Then configure 'Password settings' policy under 'AdmPwd Enterprise/Managed clients' to required values.