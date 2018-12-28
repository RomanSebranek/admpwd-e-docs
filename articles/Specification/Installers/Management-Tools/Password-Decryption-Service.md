# Password Decryption Service
Installs Password Decryption service. Service is installed to <code>%ProgramFiles%\AdmPwd\PDS</code> folder.

When installing PDS, the following actions are performed:
<ul>
 	<li>Windows Firewall exception is created, allowing AdmPwd.PDS.exe to open necessary  network listeners</li>
 	<li>Subfolder <code>CryptoKeyStorage</code> is created under <code>%ProgramFiles%\AdmPwd\PDS</code> folder, and PDS service account (NETWORK SERVICE) receives read/write permission to this folder
<ul>
 	<li>This is necessary so as PDS service could maintain encryption/decryption keys in this folder</li>
</ul>
</li>
 	<li>Registration of event message file PDS.Messages.dll for service with EventLog service</li>
 	<li>Event message file receives additional Read+Execute permission for LOCAL SERVICE. This is needed for Event Log service to be able to load the file</li>
</ul>

Unattended setup of PDS can be achieved by the following command line:  
<code>Msiexec /i /q AdmPwd.E.Tools.Setup.&lt;platform&gt;.msi ADDLOCAL=PDS</code>

<em>Note</em>: During uninstall, installer does NOT remove any key pairs contained in CryptoKeyStorage folder