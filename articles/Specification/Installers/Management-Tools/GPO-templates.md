# GPO templates
ADMX templates are installed to <code>%SystemRoot%\PolicyDefinitions</code> folder. Only English US language version of template is delivered as a part of the solution.

In case that organization uses Centralized ADMX template store , templates need to be copied to the Central store manually – the following files:
<ul>
 	<li>AdmPwd.E.admx</li>
 	<li>En-us\AdmPwd.E.adml</li>
</ul>
AMX templates deliver the following new UI to Computer part configuration of Administrative Templates GPO:
<ul>
 	<li>AdmPwd Enterprise: root container for configuration settings
<ul>
 	<li>Managed clients: configuration settings for managed clients</li>
 	<li>Administrative tools: Configuration settings for management tools</li>
</ul>
</li>
</ul>

Unattended setup of PowerShell module can be achieved by the following command line:  
<code>Msiexec /i /q AdmPwd.E.Tools.Setup.&lt;platform&gt;.msi ADDLOCAL=Management.ADMX</code>