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

User is not member of any mandatory group; returning AccessDenied error to caller.   
User: %1</td>
<td>
Logged whenever access control denies to fulfill the request because caller is not member of at least one configured mandatory group.</td>
</tr>
<tr>
<td>1007</td>
<td>Information</td>
<td>
Supported forest updated.

Forest: %1   
User: %2</td>
<td>
Logged whenever configuration of supported forest is updated.</td>
</tr>
<tr>
<td>1107</td>
<td>Warning</td>
<td>
Failed to update supported forest.

Forest: %1      
User: %2   
Error: %3
</td>
<td>
Logged whenever update of configuration of supported forest fails.</td>
</tr>
<tr>
<td>1008</td>
<td>Information</td>
<td>
Immediate reset of password of managed account requested.

Forest:%1   
Account: %2   
User:%3   
Expiration time: %4
</td>
<td>
Logged whenever immediate reset of password of Managed Domain Account is requested and PDS fulfills the request.</td>
</tr>

<tr>
<td>1009</td>
<td>Information</td>
<td>
List of supported forests retrieved.

User: %1
</td>
<td>
Logged whenever user retrieves list of supported forests via PowerShell.</td>
</tr>
<tr>
<td>1109</td>
<td>Warning</td>
<td>
Failed to get list of supported forests.

User: %1   
Error: %2
</td>
<td>
Logged whenever PDS refuses to return list of supported AD forests.</td>
</tr>

<tr>
<td>1010</td>
<td>Information</td>
<td>
Supported forest added.

Forest: %1   
User: %2
</td>
<td>
Logged whenever user adds new supported forest via PowerShell.</td>
</tr>
<tr>
<td>1110</td>
<td>Warning</td>
<td>
Failed to add supported forest.

Forest: %1   
User: %2   
Error: %3
</td>
<td>
Logged whenever PDS refuses to add new supported AD forests.</td>
</tr>

<tr>
<td>1011</td>
<td>Information</td>
<td>
Supported forest removed.

Forest: %1   
User: %2
</td>
<td>
Logged whenever user removes supported forest via PowerShell.</td>
</tr>
<tr>
<td>1111</td>
<td>Warning</td>
<td>
Failed to remove supported forest.

Forest: %1   
User: %2   
Error: %3
</td>
<td>
Logged whenever PDS refuses to remove supported AD forests.</td>
</tr>

<tr>
<td>1012</td>
<td>Information</td>
<td>
Sid mapping added.

Primary SID: %1   
Mapped SID: %2   
User: %3
</td>
<td>
Logged whenever user adds SID mapping via PowerShell.</td>
</tr>
<tr>
<td>1112</td>
<td>Warning</td>
<td>
Failed to remove SID mapping.

Primary SID: %1   
Mapped SID: %2   
User: %3   
Error: %4
</td>
<td>
Logged whenever PDS refuses to remove SID mapping.</td>
</tr>

<tr>
<td>1013</td>
<td>Information</td>
<td>
Sid mappings retrieved.

User: %1
</td>
<td>
Logged whenever user retrieves list of SID mappings via PowerShell.</td>
</tr>
<tr>
<td>1113</td>
<td>Warning</td>
<td>
Failed to retrieve SID mappings.

User: %1   
Error: %2
</td>
<td>
Logged whenever PDS refuses to return list of SID mappings.</td>
</tr>

<tr>
<td>1014</td>
<td>Information</td>
<td>
Sid mapping updated.

Primary SID: %1   
Mapped SID: %2   
User: %3
</td>
<td>
Logged whenever user updates SID mapping via PowerShell.</td>
</tr>
<tr>
<td>1114</td>
<td>Warning</td>
<td>
Failed to update SID mapping.

Primary SID: %1   
Mapped SID: %2   
User: %3   
Error: %4
</td>
<td>
Logged whenever PDS refuses to update SID mapping.</td>
</tr>

<tr>
<td>1015</td>
<td>Information</td>
<td>
Sid mapping removed.

Primary SID: %1   
User: %2
</td>
<td>
Logged whenever user removes SID mapping via PowerShell.</td>
</tr>
<tr>
<td>1115</td>
<td>Warning</td>
<td>
Failed to remove SID mapping.

Primary SID: %1   
User: %2   
Error: %3
</td>
<td>
Logged whenever PDS refuses to remove SID mapping.</td>
</tr>

<tr>
<td>1018</td>
<td>Information</td>
<td>
Managed accounts container added.

Container DN: %1   
User: %2
</td>
<td>
Logged whenever user adds Managed Accounts Container via PowerShell.</td>
</tr>
<tr>
<td>1118</td>
<td>Warning</td>
<td>
Failed to add managed accounts container.

Container DN: %1   
User: %2   
Error: %3
</td>
<td>
Logged whenever PDS refuses to add new Managed Accounts Container.</td>
</tr>

<tr>
<td>1019</td>
<td>Information</td>
<td>
Managed accounts container updated.

Container DN: %1   
User: %2
</td>
<td>
Logged whenever user updates Managed Accounts Container via PowerShell.</td>
</tr>
<tr>
<td>1119</td>
<td>Warning</td>
<td>
Failed to update managed accounts container.

Container DN: %1   
User: %2   
Error: %3
</td>
<td>
Logged whenever PDS refuses to update Managed Accounts Container.</td>
</tr>

<tr>
<td>1020</td>
<td>Information</td>
<td>
Managed accounts container removed.

Container DN: %1   
User: %2
</td>
<td>
Logged whenever user removes Managed Accounts Container via PowerShell.</td>
</tr>
<tr>
<td>1120</td>
<td>Warning</td>
<td>
Failed to remove managed accounts container.

Container DN: %1   
User: %2   
Error: %3
</td>
<td>
Logged whenever PDS refuses to remove Managed Accounts Container.</td>
</tr>
</tbody>
</table>