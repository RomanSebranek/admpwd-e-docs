# Logging
PDS records its activity into dedicated Windows log: Application and Service logs - GreyCorbel – AdmPwd.E – PDS – Operations and auditing Events logged by PDS fall into 4 categories:

| Category           | Event  types |
|--------------------|--------------|
| Service Operation  | Events that cover PDS internal opration, such as shutdown, startup, key loading, DNS registration |
| Audit              | Events that track user activity, such as password reads and resets, and creation of new key pair by KeyAdmin role |
| Licensing          | Events that describe status of licenses and license counting |
| Account management | Events that cover PDS activity in management of password of Managed Domain Accounts |

Specification of events for each category is available in the following documents:
* [Service Operation](Logging/Operational.md)
* [Licensing](Logging/Licensing.md)
* [Audit](Logging/Audit.md)
* [Managed domain accounts management](Logging/Account-management.md)

