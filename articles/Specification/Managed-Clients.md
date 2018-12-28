# Managed Clients
Management on client side is implemented as either:
* Group Policy Client Side Extension (GPO SCE) - for all platforms except for Nano Server
* Management Win32 service - on Nano server platform. This is because GPO framework is not available on Nano server

Functionality of both implementation is equivalent, and technical specification thus covers both implementation approaches. In case there are differences, they are covered in respective areas.
Management client specification covers the following areas:
* [Implementation details](Managed-Clients/Implementation.md)
* [Client configuration](Managed-Clients/Configuration.md)
* [Logging](Managed-Clients/Logging.md)

