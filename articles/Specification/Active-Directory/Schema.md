# Schema
AdmPwd.E relies on 3 new attributes added to AD schema. Attributes store: 
* Password of:
  * Managed account for each workstation (for local accounts)
  * Password of managed domain acocunts (for domain account)
* Password history
* Timestamp of password expiration.

All attributes are added to `may-contain` attribute set of `user` class.  
Specification of attributes added to AD schema by the solution is in the table below.
<table>
<thead>
<tr>
<th>Attribute</th>
<th>Parameter</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>

`ms-MCS-AdmPwd`</td>
<td>Syntax</td>
<td>2.5.5.5<br/>
(Printable case-sensitive string)</td>
</tr>
<tr>
<td></td>
<td>omSyntax</td>
<td>19</td>
</tr>
<tr>
<td></td>
<td>isSingleValued</td>
<td>True</td>
</tr>
<tr>
<td></td>
<td>searchFlags</td>
<td>904
(fCONFIDENTIAL + fPRESERVEONDELETE + fRODCFilteredAttribute + fNeverAuditValue)</td>
</tr>
<tr>
<td></td>
<td>isMemberOfPartialAttributeSet</td>
<td>False</td>
</tr>
<tr>
<td></td>
<td>OID</td>
<td>BASE_OID.2.1</td>
</tr>
<tr>
<td>

`ms-MCS-AdmPwdExpirationTime`</td>
<td>Syntax</td>
<td>2.5.5.16<br/>
(Large integer)</td>
</tr>
<tr>
<td></td>
<td>omSyntax</td>
<td>65</td>
</tr>
<tr>
<td></td>
<td>isSingleValued</td>
<td>True</td>
</tr>
<tr>
<td></td>
<td>searchFlags</td>
<td>0</td>
</tr>
<tr>
<td></td>
<td>isMemberOfPartialAttributeSet</td>
<td>False</td>
</tr>
<tr>
<td></td>
<td>OID</td>
<td>BASE_OID.2.2</td>
</tr>
<tr>
<td>

`ms-MCS-AdmPwdHistory`</td>
<td>Syntax</td>
<td>2.5.5.5<br/>
(Printable case-sensitive string)</td>
</tr>
<tr>
<td></td>
<td>omSyntax</td>
<td>19</td>
</tr>
<tr>
<td></td>
<td>isSingleValued</td>
<td>False</td>
</tr>
<tr>
<td></td>
<td>searchFlags</td>
<td>904
(fCONFIDENTIAL + fPRESERVEONDELETE + fRODCFilteredAttribute + fNeverAuditValue)</td>
</tr>
<tr>
<td></td>
<td>isMemberOfPartialAttributeSet</td>
<td>False</td>
</tr>
<tr>
<td></td>
<td>OID</td>
<td>BASE_OID.2.3</td>
</tr>
</tbody>
</table>

**BASE_OID**: 1.2.840.113556.1.8000.2554.50051.45980.28112.18903.35903.6685103.1224907

Attributes that contain password are flagged as:  
* **Confidential**: CONTROL_ACCESS permission is required to read the value of attribute, so password is better protected  
* **Preserved on delete**: value is not stripped of the tombstone, so it is possible to recover password from deleted object  
* **RODC filtered**: value is not replicated to RODC  
* **Excluded from audit**: Domain controller does not write value of attribute to Security log when detailed auditing is enabled
