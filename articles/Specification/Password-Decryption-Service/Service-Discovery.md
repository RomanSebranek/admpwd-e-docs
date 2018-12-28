# Service Discovery

PDS registers autodiscover SRV record in DNS. Record helps client tools to locate instance of PDS service. Service is supported to be running in multiple instances; each instance maintains own SRV record.

Clients use SRV record to discover instances of PDS as follows:
* At startup, client performs query for SRV record `_admpwd._tcp.<domain>`
  * domain is client's domain
* Client orders SRV records from DNS server response in ascending order by Priority to get list of discovered PDS instances, and marks each discovered instance as available.

During its lifetime, client works with list of discovered PDS instances as follows:
* For each request for PDS, client gets the connection to the instance of PDS
* Before connection is used, it is verified that instance of PDS behind the connection is responding
* If PDS instance is not responding, connection is marked as unavailable and client tries to use next connection in the list. If not responding, client marks it as unavailable and tries to connect to next service in the list, etc…
* As long as there is at least 1 service connection marked as available in the list, client uses it.
* When there are no services in the list marked as available, client performs full discovery of available services (via DNS query for PDS SRV records) and throws exception informing upper layers of application logic that there were no services available
* After receiving the exception, upper layers of application logic will decide how to continue. When decision is to retry the request, fresh list of discovered instances is used

*Note*: The same logic is used when client gets list of PDS instances from registry GPO - se [Management tools configuration](../Management-Tools/Configuration.md) for details

Decision flow is depicted on flowchart below:

![PDS discovery process](/images/PDS/pds_discovery.jpg)

This means that autodiscovery data need to be available (via GPO or DNS) in every domain that hosts machines with AdmPwd.E administrative tools installed (not managed computers)
Instance of PDS service maintains SRV record in own AD domain only by default, however, it can maintain records in other domains, too – see [PDS configuration](Configuration.md) for details.  
SRV records in DNS may be maintained manually. In this case, PDS shall be configured to mode that does not periodically register SRV records – also see [PDS configuration](Configuration.md) for details on how to turn off registration of autodiscovery SRV records.