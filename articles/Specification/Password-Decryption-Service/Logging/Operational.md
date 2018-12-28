# Operational logging
Events related to service lifetime and operations fall into **Service Operation** category and are specified in table below.
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
<td>100</td>
<td>Success</td>
<td>Service started</td>
<td></td>
</tr>
<tr>
<td>101</td>
<td>Success</td>
<td>Service stopped</td>
<td></td>
</tr>
<tr>
<td>102</td>
<td>Success</td>
<td>Autodiscover record updated.
Record: %1</td>
<td>Logged every time PDS successfully updates its DNS SRV record</td>
</tr>
<tr>
<td>202</td>
<td>Error</td>
<td>Failed to update Autodiscover record.
Record: %1
Error: %2</td>
<td>Logged in case that PDS fails to update its DNS SRV record</td>
</tr>
<tr>
<td>103</td>
<td>Success</td>
<td>Autodiscover record removed
Record: %1</td>
<td>Logged when PDS removes its DNS SRV record.
Only happens when SrvRecordUnregisterOnShutdown parameter is set to true</td>
</tr>
<tr>
<td>203</td>
<td>Error</td>
<td>Failed to remove Autodiscover record.
Record: %1
Error: %2</td>
<td>Logged in case that PDS fails to remove its DNS SRV record</td>
</tr>
<tr>
<td>104</td>
<td>Information</td>
<td>Registering autodiscover SRV record with following:
Domain: %1
Host: %2
Port: %3
Priority: %4
Weight: %5
TTL: %6</td>
<td>Logged before registration of DNS SRV record. Shows parameters of SRV record being registered.</td>
</tr>
<tr>
<td>305</td>
<td>Warning</td>
<td>Expiration time exists but password empty. This typically happens when service does not have properly configured permissions in AD.
Please verify configuration and if needed, fix permissions via Set-AdmPwdServiceAccountPermission cmdlet.
Forest: %1
Computer: %2
User: %3</td>
<td>Logged in case that service detects that response for local admin password retrieval contains timestamp of password expiration, but not a password itself.
This is to notify administrator of solution that PDS may not have enough permissions to read password from AD</td>
</tr>
<tr>
<td>106</td>
<td>Information</td>
<td>Key Pair loaded
Id: %1</td>
<td>Logged when PDS successfully loads key pair from keystore
<em>Note</em>: Custom implementations of keystore are expected to implement logging of this event.</td>
</tr>
<tr>
<td>207</td>
<td>Error</td>
<td>File based keystore does not exist. No keys will be loaded.
Keystore folder: %1</td>
<td>

Logged when PDS does not find keystore folder pointed to by Pds – KeyStore – path parameter (see [PDS configuration](../Configuration.md) for details)</td>
</tr>
<tr>
<td>208</td>
<td>Error</td>
<td>Error during Autodiscover registration/unregistration.
Error: %1</td>
<td>Logged when PDS encounters unexpected error during SRV record registration/deregistration</td>
</tr>
<tr>
<td>209</td>
<td>Error</td>
<td>Could not load information about forest %1.
Error: %2
Forest will be ignored until next service start.</td>
<td>Logged when PDS is not able to reach supported forest</td>
</tr>
<tr>
<td>210</td>
<td>Error</td>
<td>Could not get ACL of directory object; returning AccessDenied to caller.
Object DN: %1</td>
<td>Logged when PDS is not able to reach DC of forest where PDS is installed</td>
</tr>
<tr>
<td>211</td>
<td>Error</td>
<td>Could not load information about implicit local forest.
Error: %1
Please fix the error and restart the service to retry.</td>
<td>Logged when PDS is not able to get ACL for managed machine or Managed Domain Account</td>
</tr>
<tr>
<td>212</td>
<td>Error</td>
<td>

Could not open listener for service %1.<br/>
Error: %2<br/>
Verify that service port is not occupied by other program.<br/>
When PDS is co-hosted with DNS server, then DNS server may occupy many UDP ports including PDS UDP port used for centralized reporting. 
If this is a case, you can make the port available by the following PowerShell commands:<br/>
``` PowerShell
$Setting=Get-DnsServerSetting -All
$Setting.SocketPoolExcludedPortRanges='61184-61184'
Set-DnsServerSetting $Setting
```
</td>
<td>

Logged when PDS is not able to open listener on configured TCP or UDP port

*Notes*:
Service name can be one of following:
* **PDS**: For PDS core service that listens on 61184/tcp by default
* **CentralizedReporting**: For clientralized client reporting service that listens on 61184/udp by default

</td>
</tr>
</tbody>
</table>