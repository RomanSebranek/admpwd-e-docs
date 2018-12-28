# Centralized logging of client activity
## Overview
In large organizations with many managed machines, it may be a difficult task to keep all managed machines in shape in terms of:
* delivery of client runtime to machine
* delivery of GPO settings to machines
* knowing of any error that may occur on the machine during management of password of local admin and othe rtasks
* knowing that users with administrative privileges on machine use managed local admin account a wrong way, i.e. reset it's password manually

Centralized logging of client activity provides consolidated view of state of managed machines. When turned on via registry policy (delivered by GPO, DSC or by other means), management runtime sends important events to configured central server - machine with installed PDS role.
Central server stores events received from managed clients in dedicated event log `GreyCorbel-AdmPwd.E-ClientActivity/Operational`

## Network communication
Events are transported via UDP transport which is very effective in terms of network bandwidth and resource consumption. Defrault port that PDS server listens on for events from client activity is 61184/udp
*Note*: In some cases, such as when PDS is co-hosted with Windows DNS server, it may happen that default port is occupied by other service, and PDS cannot open listener on configured port. If this happens, there are the following ways to remove the conflict:
* Configure other service to use different port, or keep the default port available for PDS
* Configure PDS to listen on different port

For example, if the service that opens listener on port 61184/udp is DNS server, you can configure DNS Server service to avoid this port with simple PowerShell script:
```Powershell
$Setting=Get-DnsServerSetting -All
$Setting.SocketPoolExcludedPortRanges='61184-61184'
Set-DnsServerSetting $Setting
```

To configure PDS to use different port for listener for client activity events, change appropriate setting in `AdmPwd.PDS.exe.config` file:
```xml
<service name="AdmPwd.PDS.ClientReportingService" behaviorConfiguration="AdmPwdService.ServiceBehavior">
    <endpoint address="" binding="udpBinding" bindingNamespace="http://schemas.greycorbel.com/services/AdmPwdPDS" 
        bindingConfiguration="clientReportingBinding" contract="AdmPwd.PDS.IClientReportingService" />
    <host>
        <baseAddresses>
            <!--Centralized Reporting listener. Change port here to different one than 61184 if needed-->
            <add baseAddress="soap.udp://localhost:61184/ClientReportingService"/>
        </baseAddresses>
    </host>
</service>
```
*Note*: When changing UDP port in PDS configuration file, remember to perform the same change in registry policy - see `ReportingPort` parameter in [Managed Clients configuration](../Managed-Clients/Configuration.md)

## Forwarded events
When centralized reporting is turned on for managed clients, clients report the following events to reporting server:
* Successful changes of password of managed local admin account
* All warning events
* All report events

Reported events from clients are written to client activity log on PDS server as specified below:
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
<td>Informational</td>
<td>

Operation performed on client.  
Computer: %1  
Operation: %3
</td>
<td>

Logged when client successfully performs operation, such as password change of managed local admin password.
</td>
</tr>
<tr>
<td>1001</td>
<td>Error</td>
<td>

Error reported on client.  
Computer: %1  
Operation: %3  
Error: %4
</td>
<td>

Logged when client encounters an error during processing of management cycle.
</td>
</tr>
<tr>
<td>1002</td>
<td>Warning</td>
<td>

Warning reported on client.  
Computer: %1  
Warning: %3 (%4)  
</td>
<td>

Logged when client finds suspicious state on managed client and is going to fix it. It may be password of managed local admin account changed manually, built-in admin account in different state than required by policy, etc.
</td>
</tr>
</tbody>
</table>

*Notes*:
* All events also contain IP address from report was received. This IP address is seen only in Details/XML view of event and is not shown in user-friendly event view.
* On managed clients, events are sent to central store on best-effort basis. Failure to send the event to central store does not stop further processing.
