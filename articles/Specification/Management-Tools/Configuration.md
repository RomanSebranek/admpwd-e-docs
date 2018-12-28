# Configuration
In default configuration of solution, management tools do not require specific configuration to be able to communicate with PDS – PDS is automatically discovered using discovery process as described [here](../Password-Decryption-Service/Service-Discovery.md)

However, there may be deployment scenarios that require configuration of management tools. Management tools are configurable via configuration values stored in the following registry path:

<code>HKLM\Software\Policies\Microsoft Services\AdmPwd</code>

Currently the following configuration values are supported:
<table>
<thead>
<tr>
<th width="20%">Value</th>
<th width="20%">Type</th>
<th>Meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td>UseSharedSPN</td>
<td>REG_DWORD</td>
<td>Setting to non-zero causes management tools to use SPN SVC/AdmPwd when authenticating with PDS
When set to zero or not present at all, management tools use SPN HOST/ when authenticating with the service.
See [PDS configuration](../Password-Decryption-Service/Configuration.md) for more details on when and how to configure PDS to use specific SPN.
Managed by policy <em>“PDS service runs using domain account”</em></td>
</tr>
<tr>
<td>PDSList</td>
<td>REG_MULTI_SZ</td>
<td>List of PDS instances to be used by administrative tools. When this value is specified, management tools do not USE DNS SRV records to discover PDS instances, and rather use instances specified here.
Supported format of values:
&lt;Host FQDN&gt;(Example: host.domain.com)
&lt;Host FQDN&gt;:&lt;Port&gt; (Example: host.domain.com:61185)The latter format can be used when PDS does not listen on default tcp port (61184).
PDS instances are used in order specified in the value
Managed by policy <em>“PDS to be used”</em></td>
</tr>
</tbody>
</table>
<em>Note</em>: In GPO UI, all configuration settings related to configuration of CSE ale located under <code>Computer configuration/Administrative Templates/AdmPwd Enterprise/Administrative Tools</code> path
