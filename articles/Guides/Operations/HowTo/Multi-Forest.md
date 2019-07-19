# How to configure PDS to manage passwords in remote forests
Some organizations operate complex AD infrastructure that consists of more than one AD forest. Examples of such infrastructure include: 
- AD forests deployed in DMZs to manage extranet infrastructure
- Red forests dedicated for management infrastructure
- Hosted environments

Complexity is further increased by specific requirements for inter-forest traus relationships: there may be two-way trusts, one-way trusts or no trusts at all.

Typical deployment of PDS service is to have instance of service in AD forest that is managed by it. Setup is pretty much straghtforward:
- extend AD schema in your forest
- configure delegation model for managed machine, PDS and users of the solution
- install PDS on chosen member server(s) or domain controller(s) in the forest

And you're ready to start using the solution.

However, in multi-forest deplyoment, it is not always practical to operate instance of PDS in each forest - you may want to operate one 'central' PDS instance that takes care of multiple AD forests. Establishing this topology may not be so straightforward. This article describes various supported multi-forest topologies supported by AdmPwd.E, along with configuration procedures.

## Terminology
**Remote forest** Means AD forest that is going to be managed by PDS from different forest   
**PDS forest** Means AD forest where PDS instance is installed

## General considerations
### AD schema
Each AD forest has its own AD schema. Thus, if you want to have your password managed in the forest, you must extend the schema of that AD forest.
Best practice is to extend the AD schema via `Update-AdmPwdADSchema` cmdlet from AdmPwd.PS module. Just install AdmPwd.E (even temporarily) to suitable machine in the forest, log on as Schema and Enterprise admin, and run the cmdlet.

Alternatively, you can extend the AD schema manually via LDF files we provide on [GitHub](https://github.com/GreyCorbel/admpwd-e/tree/master/ADSchema). Import both LDF files using the script `Import-Schema.cmd` script that's on GitHub as well.

_Note_: You must run the script on machine where is `ldifde.exe` tool installed (typically on domain controller)

### Permissions for managed machines to report password to AD
Permissions for managed machines to report password to AD are always that same and are configured in the home forest of managed machine. Simply run `Set-AdmPwdComputerSelfPermission` cmdlet on OUs where managed machines reside. Again, this required AdmPwd.PS module to be installed on suitable machine in the forest you are configuring.

### Location of user accounts of users of solution
All scenarios below expect that users of the solution have their user accounts located in PDS forest. This is typical scenario for DMZ or Red forest topologies.   
Other topologies with different user accounts locations may be supported as well depending on demand - feel free to ask.

## Remote forest with 2-way trust with PDS forest
In this topology, managed machines and managed user accounts are in both PDS forest and Remote forest.   
Users of the solution (those who need to read/reset passwords) are in PDS forest only.   
PDS and remote AD forests have 2-way trust relationship established.

Configuration of permission model:
- PDS permissions in both forests are granted to PDS identity (typically 'PDS servers' group from PDS forest that contains computer accounts of PDS servers)
    - you use familiar cmdlet `Set-AdmPwdPDSPermission` for delegation
- Read Admin Password and Reset Admin Password permissions in both forests are granted to 'Password Readers' and 'Password Resetters' group from PDS forest; groups contain user accounts of users who are granted respective permission
    - you use familiar cmdlets `Set-AdmPwdReadPasswordPermission` and `Set-AdmPwdResetPasswordPermission` for delegation

For users to be able to specify Remote forest when retrieving/resetting password, you must add DNS name of Remote forest to PDS configuration as new supported forest. You do so via cmdlet as follows:   
`Get-AdmPwdPds | Add-AdmPwdPdsSupportedForest -ForestName <DNS name of Remote forest>`   
You run this command in PDS forest. You must be member of PDS Adminisrators role to be able to successfully run this command. This command discovers all instances of PDS in the PDS  forest and registers Remote forest in configuration of PDS.

## Remote forest with 1-way trust with PDS forest
In this topology, Remote forest trusts PDS forest, so security principals from PDS forest can be assigned permissions in Remote forest. Permissions model for this topology is basically the same as for 2 way trust - see above.

## Remote forest without trust with PDS forest
In this topology, managed machines and user accounts are in both PDS forest and Remote forest.   
Users of the solution (those who need to read/reset passwords) are in PDS forest only.   
There is not rust relationship between PDS forest and Remote forest.

For PDS forest, there are no specific configuration steps to establish peermission model - you just follow standard permission model setup steps.

For remote forest, you setup permission model as if the PDS was supposed to be there and running under domain account, thus:
1. Create service account for PDS in Remote forest and grant it required permissions via `Set-AdmPwdPdsPermission` cmdleet
2. Create PDS Readers security group in Remote forest and grant it required permissions via `Set-AdmPwdReadPasswordPermission` cmdlet
3. Create PDS Resetters security group in Remote forest and grant it required permissions via `Set-AdmPwdResetPasswordPermission` cmdlet
4. Find out SID of Password Readers and Password Resetters security groups in **Remote forest** and note it
You can use PowerShell command similar to below to find out SID of security group - run it on machine in Remote forest
```PowerShell
$DomainName = 'NetBIOS name of your REMOTE DOMAIN'
$GroupName = "Password Readers"
(new-object System.Security.Principal.NTAccount($DomainName, $GroupName)).Translate([System.Security.Principal.SecurityIdentifier]).Value
```
5. Find out SID of Password Readers and Password Resetters security groups in **PDS forest** and note it
    - you can use the same PowwerShell script as above to find out the SIDs, just enter proper domain name and group name and run it on machine in PDS forest
6. In PDS forest, update variables for SIDs in the commands below and run them on machine in PDS forest.
    - You must be member of PDS Admins role to successfully run the commands
``` Powershell
# Enter the user name and password of service account you created in Step 1
$ConnectionCredential - Get-Credential
$ForestName = 'DNS NAME OF REMOTE FOREST'
#Configure Remote forest as supported forest on PDS and specify connection credentials
Get-AdmPwdPDS | Add-AdmPwdPdsSupportedForest -ForestName $ForestName -Credential $ConnectionCredential
#Configure SID mappings for password reading
$PrimarySid = 'SID of Password Readers group in Remote forest'
$MappedSid = 'SID of Password Readers group in PDS forest'
$Description = 'Mapping of Password Readers from Remote forest'
Get-AdmPwdPds | Add-AdmPwdPdsSidMapping -PrimarySid $PrimarySid -MappedSid $MappedSid -Description $Description
$PrimarySid = 'SID of Password Resetters group in Remote forest'
$MappedSid = 'SID of Password Resetters group in PDS forest'
$Description = 'Mapping of Password Resetters from Remote forest'
Get-AdmPwdPds | Add-AdmPwdPdsSidMapping -PrimarySid $PrimarySid -MappedSid $MappedSid -Description $Description
```
After running the commands above, members of Password Readers group in PDS forest will also be able to read passwords on local admin accounts and managed domain accounts from Remote forest. And similarly, members of Password Resetters group in PDS forest will be able to request reset of passwords of computer local admins and managed domain accounts in Remote forest.

