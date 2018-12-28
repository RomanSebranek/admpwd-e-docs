# Installers
The following installers are delivered with the solution:
<ul>
 	<li>CSE installer: AdmPwd.E.CSE.Setup.&lt;platform&gt;.msi</li>
 	<li>Management tools installer: AdmPwd.E.Tools.Setup.&lt;platform&gt;.msi</li>
</ul>
Specifics for each installer are summarized in respective sections:

* [Management runtime](./Installers/Managed-Clients.md)
* [Management tools](./Installers/Management-Tools.md)
  * [Fat client UI](./Installers/Management-Tools/Fat-Client.md)
  * [GPO Templates](./Installers/Management-Tools/GPO-Templates.md)
  * [PowerShell Module](./Installers/Management-Tools/PowerShell-Module.md)
  * [Password Decryption Service](./Installers/Management-Tools/Password-Decryption-Service.md)

All installers are packaged as MSI packages. Standard Windows Management runtime is packaged as dedicated MSI (to minimize size and avoid distribution of unnecessary component installer to managed machines); Management tools are packaged in another MSI.

Nano Server management runtime  package is delivered as AppX package, with configuration settings implemented as PowerShell DSC configuration.
