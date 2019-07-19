# Licensing
Events related to license validation fall into **Licensing** category and are specified in table below.
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
<td>2100</td>
<td>Warning</td>
<td>License file not found. Running in freeware mode.
License file path: %1</td>
<td>Logged when administrator did not provide license file. PDS runs in freeware mode that allows usage for 25 managed machines</td>
</tr>
<tr>
<td>2000</td>
<td>Information</td>
<td>

License validated.  
Forest: %1  
Licensed computers: %2  
License usage: %3 (%4%)  
License expiration: %5
</td>
<td>

Logged after successful validation of license file.  
Placeholders used:  
%1 - AD forest where PDS is installed  
%2 – number of licensed computers  
%3 – number of computers managed by solution  
%4 – percentage of license usage  
%5 – when current license expires
</td>
</tr>
<tr>
<td>2001</td>
<td>Information</td>
<td>

Running in freeware mode.  
Licensed computers: %1  
License usage: %2 (%3%)
</td>
<td>

Logged after determining that solution is running without license file in freeware mode.  
Placeholders used:  
%1 – number of licensed computers by freeware license (currently 25)  
%2 – number of computers managed by solution  
%3 – percentage of license usage
</td>
</tr>
<tr>
<td>2002</td>
<td>Information</td>
<td>

Managed machines stats for domain.  
Domain: %1  
License usage: %2</td>
<td>Logged for every domain covered by the solution. Shows number of machines managed by solution in each domain.</td>
</tr>
<tr>
<td>2003</td>
<td>Information</td>
<td>

Add-on license validated.  
Add-on: %1  
Type: %2</td>
<td>Logged when license for add-on component is successfully validated
Currently only add-on component that requires separate licensing is HSM keystore</td>
</tr>
<tr>
<td>2004</td>
<td>Information</td>
<td>

Running in freeware mode.  
Licensed managed accounts: %1  
License usage: %2 (%3%)</td>
<td>Logged when license file is not found, does not contain license for Managed Domain Accounts, license file is tampered with, or license is expired</td>
</tr>
<tr>
<td>2101</td>
<td>Warning</td>
<td>

License is about to expire.  
Expiration date: %1  
Please install updated license file.</td>
<td>Logged when license has 30 days or less to expiration date.</td>
</tr>
<tr>
<td>2102</td>
<td>Warning</td>
<td>

License file not found. Running in freeware mode.  
License file path: %1</td>
<td>Logged when license file specified in PDS config file is not found</td>
</tr>
<tr>
<td>2103</td>
<td>Warning</td>
<td>

Missing license for add-on. Add-on switched to freeware mode.  
Add-on: %1  
Type: %2</td>
<td>Logged when license file does not contain license for add-on component.

Currently only add-on component that requires separate licensing is HSM keystore</td>
</tr>
<tr>
<td>2201</td>
<td>Error</td>
<td>

License file found, but digital signature validation did not pass. Running in freeware mode.  
Please download updated license.  
License file path: %1
</td>
<td>

Logged when PDS finds out that license file was tampered with – digital signature is invalid. In this situation, PDS switches to freeware mode.
Administrator shall immediately download fresh copy of license file from [AdmPwd licensing web](https://licensing.admpwd.com).
</td>
</tr>
<tr>
<td>2202</td>
<td>Error</td>
<td>License expired. Running in freeware mode.
Expiration date: %1</td>
<td>

Logged when PDS finds out that license has expired. Administrator shall immediately download fresh copy of license file from [AdmPwd licensing web](https://licensing.admpwd.com).</td>
</tr>
<tr>
<td>2203</td>
<td>Error</td>
<td>

Not enough licenses. For some computers, you may not be able to retrieve/reset admin password.  
Licensed computers: %1  
Total licenses needed: %2
</td>
<td>Logged when PDS finds out that solution manages more computers than currently licensed.
When this happens, PDS may randomly refuse to get/reset password for a computer.  

Probability of refusing an operation increases with number of unlicensed computers  
*Note*: Probability is computed as follows:  
`# of unlicensed computers / # of all computers`</td>
</tr>
<tr>
<td>2204</td>
<td>Error</td>
<td>

License issued for different AD forest. Running in freeware mode.  
Licensed forest: %1  
Current forest: %2</td>
<td>

This event is logged when PDS finds out that it runs in different AD forest than licensed.
License is issued for specific AD forest where PDS is installed in.  
*Note*: PDS can support multiple AD forests, but it needs to have license for own forest and enough of licensed machines and managed domain accounts all forests where managed computers and domain accounts are.  
When there is not license file for PDS local forest, PDS switches into freeware mode until license for proper forest is provided by an administrator.
</td>
</tr>
<tr>
<td>2205</td>
<td>Error</td>
<td>

Error during license validation. Running in freeware mode.  
License file: %1  
Error: %2
</td>
<td>

Logged when PDS encounters any other error during license validation than specified otherwise.  
When this happens, PDS switches into freeware mode.</td>
</tr>
<tr>
<td>2206</td>
<td>Error</td>
<td>

Error during license usage counting.  
Domain: %1  
Error: %2</td>
<td>Logged when PDS is not able to count license usage in specific domain
When this happens, PDS temporarily allows to continue and does not count the forest into license consumption, but does not serve password read and reset requests for affected AD forest.</td>
</tr>
<tr>
<td>2207</td>
<td>Error</td>
<td>

Error during license usage counting.  
Error: %1</td>
<td>

Logged when PDS encounters unexpected error when counting license usage.  
When this happens, PDS temporarily allows to continue and does not count the forest into license consumption, but does not serve password read and reset requests for affected AD forest.</td>
</tr>
<tr>
<td>2208</td>
<td>Error</td>
<td>

Not enough licenses for managed domain user accounts. For some managed accounts, you may not be able to retrieve/reset password.  
Licensed managed accounts: %1  
Total licenses needed: %2</td>
<td>Logged when PDS finds out that there is not enough licenses for all Managed Domain Accounts found in environment</td>
</tr>
</tbody>
</table>