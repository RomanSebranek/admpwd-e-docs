# Managed clients
## MSI Installer
MSI installer installs client side GPO extension that maintains password of local admin account. CSE is installed to <code>%ProgramFiles%\AdmPwd\CSE</code> folder.

In addition, during CSE installation, the following actions can be performed:
<ul>
 	<li>Creation of custom local admin account
<ul>
 	<li>Account receives cryptographically random, complex password 16 characters long, and is made member of local Administrators group</li>
 	<li>Installer performs this action when value of property <code>CUSTOMADMINNAME</code> is set to value other than “__null__”</li>
 	<li>Value can be passed from command line, or via MST file created by tool of choice (such as Orca )</li>
 	<li>Example of command line that installs CSE and creates custom local admin account named CustomAdmin: <code>msiexec /i /q AdmPwd.E.CSE.Setup.x64.msi CUSTOMADMINNAME=CustomAdmin</code></li>
</ul>
</li>
 	<li>Reset password of built-in local admin account
<ul>
 	<li>Account receives cryptographically random, 16 characters long, complex password</li>
 	<li>Installer performs this action when value of property <code>PROTECTBUILTINADMIN</code> is set to “true”</li>
</ul>
</li>
</ul>

## AppX installer for Nano
AppX installer installs management service, plus event manifest for registration of event publisher for logging to Application Event log. Additional features like creation of custom admin account and protection of built-in admin account are not available for installer on Windows Nano Server platform.
Sample configuration is delivered in AdmPwd.E.Config.ps1 file that is part of download with Nano server install package.