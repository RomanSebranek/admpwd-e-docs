# Active Directory
AdmPwd.E uses Active DIrectory for the following purpose:
* As storage for passwords on managed admin accounts (local accounts on managed machines, or managed domain accounts)
* As authentication and authorization platform
* As provider of encryption mechanisms to protect transmitted data over the network

To be able to provide password storage, AdmPwd.E comes with extension of AD schema. Details for AD schema extensions required by solution are provided in section [AD Schema changes](Active-Directory/Schema.md)

To be able to provide support for authorization decisions, AdmPwd.E comes with solution-specific Extended Rights that are registered to AD. Details on new Extended Rigths are provided in section [AD Extended Rights](Active-Directory/Extended-Rights.md)

## Information security
AdmPwd.E maintains 3 pieces of information for managed accounts in Active Directory:
* Current password
* Timestamp of expiration of current password
* Password history (optional)

Solution establishes security model around information stored in Active Directory. Model is defined as follows:  
<table>
<thead>
<tr>
<th>Information</th>
<th>Who needs to read</th>
<th>Who needs to write</th>
</tr>
</thead>
<tbody>
<tr>
<td>Password</td>
<td>PDS identity</td>
<td>

*Local admin accounts*: Computer that owns the computer account (so every computer can write password of own admin account to AD)  

*Managed domain accounts*: PDS identity (PDS manages password of all managed domain account)  
*Note*: For managed domain accounts, PDS also requires to have permission to reset password of managed account
</td>
</tr>
<tr>
<td>Password Expiration Timestamp</td>
<td>PDS identity  

Computer that owns the computer account (so every computer can know whether it is the time to change the password of own admin account)</td>
<td>

*Local admin accounts*:  
* Computer that owns the computer account (so every computer can write password expiration of own admin account to AD)  
* PDS identity

*Managed domain accounts*: PDS identity (PDS manages password of all managed domain account and writes also their expiration timestamp)  
</td>
</tr>
<tr>
<td>Password history</td>
<td>PDS identity</td>
<td>

*Local admin accounts*:
* Computer that owns the computer account (so every computer can write password history of own admin account to AD)  
* AD administrator (to be able to maintain password history)

*Managed domain accounts*: PDS identity (PDS manages password of all managed domain account, including password history)  
</td>
</tr>
</tbody>
</table>

*Note*: Domain administrators can obviously read and write all attributes (or give themselves permission to do so), but – in case that password is stored encrypted in AD – they still are not able to get decrypted password unless given explicit permission. Only PDS can decrypt the password stored in AD. This means that solution provides certain form of security model independent on AD infrastructure security model. This is unlike permission model in [Microsoft LAPS](https://technet.microsoft.com/en-us/mt227395.aspx) solution that purely relies on AD security model.

## Network communication
Communication of AdmPwd.E components with Active Directory includes:
* Reporting of password from managed machines to attributes of computer account in AD
* Password reset initiated by PDS on managed domain accounts
* Reporting of password of managed domain account to attributes of managed domain account in AD
* Retrieval of password of managed local account or managed domain account by PDS when eligible users request reading of the password


In all above scenarios, all transferred information is encrypted by Kerberos encryption, protecting transmitted data from eavesdropping. This is generally the same encryption that protects information on Kerberos tokens issued by domain controllers.

## Protection against deletion of computer account
Computer accounts might be subject of accidental deletion. In such case (especially when AD Recycle Bin feature of Windows 2008 R2 is not implemented) password of managed local admin account would be lost and there would not be an easy way for support staff to read it: it would require using the SystemState backup to read the password – unless the Forest Functional Level (FFL) is Windows 2008 R2 and AD Recycle Bin feature is turned on. Approach for protection against accidental deletion of computer account is implemented as follows:
* <code>ms-MCS-AdmPwd</code> and <code>ms-MCS-AdmPwdHistory</code> attributes are added to the set of attributes that will not be stripped off the object during the deletion
* This means that password will still be available on tombstone of computer account for the lifetime of tombstone – which is 180 days by default
* So, when accidental deletion of computer account occurs, Domain admin role will be able to quickly reanimate the tombstone object, and password can be retrieved from reanimated tombstone using solution client tools (such as Web UI)
* Only after tombstone expires, the password is lost.

Tombstone lifetime is long enough for password recovery of accidentally deleted computer object.   
Main benefit of this approach is that it allows not exporting passwords from AD infrastructure to independent location where it would need to be specially protected – just for the sake of protection against the special case of accidentally deleted computer account.

In addition, solution comes with ability to retrieve password directly from deleted computer account. Prerequisite for this ability is to properly set permission on Deleted Objects container in each domain of forest:
* Read Property
* List

Administrators can use cmdlet <code>Set-AdmPwdPdsDeletedObjectsPermission</code> to delegate the permission of <code>Deleted Objects</code> container to PDS service account.

*Note*: Deleted computer object keeps its original ACL – this means that users who were delegated to retrieve password from computer object before its deletion can do this on deleted computer, too.
