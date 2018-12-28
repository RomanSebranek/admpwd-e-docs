# Audit
Events related to activity of users of solution fall into **Audit** category and are specified in table below.
<table>
<thead>
<tr>
<th width="8%">ID</th>
<th width="15%">Severity</th>
<th>Description</th>
<th>Comment</th>
</tr>
</thead>
<tbody>
<tr>
<td>1000</td>
<td>Information</td>
<td>

Admin password retrieved.  
Forest: %1  
Computer: %2  
IsDeleted: %3  
User: %4</td>
<td>

Logged when User retrieves password for admin account of specific computer in given forest.  
Empty Forest field means that local PDS forest was used.  
IsDeleted field is Boolean value that is true when user retrieved password for deleted computer account</td>
</tr>
<tr>
<td>1100</td>
<td>Warning</td>
<td>

Failed to retrieve admin password.  
Forest: %1  
Computer: %2  
IsDeleted: %3  
User: %4  
Error: %5</td>
<td>

Including scenario when user requesting the password retrieval does not have permission granted</td>
</tr>
<tr>
<td>1001</td>
<td>Information</td>
<td>

Admin password reset.  
Forest: %1  
Computer: %2  
User: %3  
Expiration time: %4</td>
<td>

Expiration time contains expiration time specified by user in request.  
For immediate expiration, current time is sent.</td>
</tr>
<tr>
<td>1101</td>
<td>Warning</td>
<td>

Failed to reset admin password.  
Forest: %1  
Computer: %2  
User: %3  
Error: %4</td>
<td>

Including scenario when user requesting the password reset does not have permission granted</td>
</tr>
<tr>
<td>1002</td>
<td>Information</td>
<td>

Key pair generated.  
KeyID: %2  
User: %1</td>
<td></td>
</tr>
<tr>
<td>1102</td>
<td>Warning</td>
<td>

Failed to generate key pair.  
User: %1  
Error: %2</td>
<td>

Including scenario when user requesting key pair generation is not member of PDS Admin role</td>
</tr>
<tr>
<td>1003</td>
<td>Information</td>
<td>

Password of managed account retrieved.  
Forest: %1  
Account: %2  
User: %3</td>
<td>

Logged when User retrieves password for specified Managed Domain Account in given forest.  
Empty Forest field means that local PDS forest was used.</td>
</tr>
<tr>
<td>1103</td>
<td>Warning</td>
<td>

Failed to retrieve password of managed account.  
Forest: %1  
Account: %2  
User: %3  
Error: %4</td>
<td>

Including scenario when user requesting the password retrieval does not have permission granted</td>
</tr>
<tr>
<td>1004</td>
<td>Information</td>
<td>

Password of managed account reset.  
Forest: %1  
Account: %2  
User: %3  
Expiration time: %4</td>
<td>

Expiration time contains expiration time specified by user in request.  
For immediate expiration, current time is sent.</td>
</tr>
<tr>
<td>1104</td>
<td>Warning</td>
<td>

Failed to reset password of managed account.  
Forest: %1  
Account: %2  
User: %3  
Error: %4</td>
<td>

Including scenario when user requesting the password reset does not have permission granted</td>
</tr>
<tr>
<td>1105</td>
<td>Information</td>
<td>

SID mapped.  
User: %1  
Primary SID: %2  
Mapped SID: %3  
Description: %4</td>
<td>

Logged whenever access control decision was made based on SID mapping.</td>
</tr>
<tr>
<td>1106</td>
<td>Information</td>
<td>

User is not member of any mandatory group; returning AccessDenied to caller.  
User: %1</td>
<td>

Logged whenever access control denies to fulfill the request because caller is not member of at least one configured mandatory group.</td>
</tr>
</tbody>
</table>