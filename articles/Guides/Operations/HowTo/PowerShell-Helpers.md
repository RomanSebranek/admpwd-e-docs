# PowerShell scripts and helpers
This article shows various PowerShell one-liners, scripts and helpers that can help you operate the solution.

Commands often use Powershell module S.DS.P for searching AD. Module can be downloaded from [GitHub](https://github.com/jformacek/S.DS.P)

## List computers that report password to AD
This script lists all computer accounts in domain that are managed by solution
```powershell
#import PowerShell module
Import-Module S.DS.P
#establish LDAP connection to a domain controller from your domain
$conn=Get-LdapConnection -LdapServer $env:LogonDomain
#get domain controller parameters
$rootDSE=Get-RootDSE -LdapConnection $conn
#load computer objects that have password expiration populated; conver password expiration to datetime and sort ascending by password expiration
Find-LdapObject -LdapConnection $conn -searchFilter '(&(objectClass=computer)(ms-Mcs-AdmPwdExpirationTime=*))' -searchBase $rootDSE.defaultNamingContext -PropertiesToLoad 'cn','ms-Mcs-AdmPwdExpirationTime' | %{$_.'ms-Mcs-AdmPwdExpirationTime'=[DateTime]::FromFileTimeUtc($_.'ms-Mcs-AdmPwdExpirationTime');$_} | select cn,ms-Mcs-AdmPwdExpirationTime | sort ms-Mcs-AdmPwdExpirationTime
```

## Get password update events from client activity log
This command looks for password update events in centralized client activity log.
```powershell
#filter event log to list password update events, and report computername and timestamp when event was logged
$data=Get-WinEvent -LogName 'GreyCorbel-AdmPwd.E-ClientActivity/Operational' -FilterXPath "*[EventData[Data='Admin password updated']]" | select -property @{N='Computer';E={$_.Properties[0].Value}}, @{N='Timestamp';E={$_.TimeCreated}}
```

