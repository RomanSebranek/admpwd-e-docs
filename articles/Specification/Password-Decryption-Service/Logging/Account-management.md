# Account Management
Events related to management of password of domain accounts fall into **Account Management** category and are specified in table below.

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
<td>3000</td>
<td>Information</td>
<td>

Password of managed account was updated.  
Account: %1</td>
<td>

Logged when PDS updates password of Managed Domain Account</td>
</tr>
<tr>
<td>3002</td>
<td>Information</td>
<td>

Password history of managed account was updated.  
Account: %1  
Passwords preserved: %2</td>
<td>

Logged when PDS updates password history of Managed Domain Account - shrinks amount of values according to configured policy</td>
</tr>
<tr>
<td>3200</td>
<td>Warning</td>
<td>

Password of managed account could not be updated.  
Account: %1  
Error: %2
</td>
<td>

Logged when PDS is not able to update password of Managed Domain Account</td>
</tr>
<tr>
<td>3201</td>
<td>Warning</td>
<td>

Error during password management cycle of managed accounts.  
Error: %1</td>
<td>

Logged PDS encounters unexpected error during update of password of Managed Domain Accounts</td>
</tr>
<tr>
<td>3202</td>
<td>Warning</td>
<td>

Password history of managed account could not be updated.  
Account: %1  
Error: %2</td>
<td>

Logged when PDS is not able to manage content of password history of Managed Domain Accounts</td>
</tr>
</tbody>
</table>