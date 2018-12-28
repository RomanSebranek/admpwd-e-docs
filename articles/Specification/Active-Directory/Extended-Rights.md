# Extended Rights

Solution defines 2 new extended rights in AD Configuration partition. Extended rights are used by PDS to authorize password read and reset requests of users: user has to be granted respective permission to perform the password read/reset.  
By default, the rights are not assigned to anyone (not even to Domain/Enterprise admins) and must be explicitly assigned so as users have ability to read/reset password of managed accounts.

 Specification is in table below.
<table>
<thead>
<tr>
<th>Right</th>
<th>Parameter</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>

`ms-Mcs-AdmPwdReadPassword`</td>
<td>objectClass</td>
<td>controlAccessRight</td>
</tr>
<tr>
<td></td>
<td>displayName</td>
<td>Read Administrator Password</td>
</tr>
<tr>
<td></td>
<td>appliesTo</td>
<td>
bf967a86-0de6-11d0-a285-00aa003049e2<br/>
(computer objects)<br/>
bf967aba-0de6-11d0-a285-00aa003049e2<br/>
(user objects)
</td>
</tr>
<tr>
<td></td>
<td>rightsGuid</td>
<td>2a72352f-f5f8-40a3-83b2-1d8562fa90c4</td>
</tr>
<tr>
<td></td>
<td>validAccesses</td>
<td>256
See <a href="http://blogs.msdn.com/b/openspecification/archive/2009/08/19/active-directory-technical-specification-control-access-rights-concordance.aspx" target="_blank" rel="noopener noreferrer">here</a> for details</td>
</tr>
<tr>
<td></td>
<td>showInAdvancedViewOnly</td>
<td>FALSE</td>
</tr>
<tr>
<td>

`ms-Mcs-AdmPwdResetPassword`</td>
<td>objectClass</td>
<td>controlAccessRight</td>
</tr>
<tr>
<td></td>
<td>displayName</td>
<td>Reset Administrator Password</td>
</tr>
<tr>
<td></td>
<td>appliesTo</td>
<td>
bf967a86-0de6-11d0-a285-00aa003049e2<br/>
(computer objects)<br/>
bf967aba-0de6-11d0-a285-00aa003049e2<br/>
(user objects)
</td>
</tr>
<tr>
<td></td>
<td>rightsGuid</td>
<td>5E4DF2BA-49FB-4703-87D9-B69F00C4C039</td>
</tr>
<tr>
<td></td>
<td>validAccesses</td>
<td>256</td>
</tr>
<tr>
<td></td>
<td>showInAdvancedViewOnly</td>
<td>FALSE</td>
</tr>
</tbody>
</table>