# PowerShell Module
PowerShell module name is AdmPwd.PS. It’s installed to <code>$pshome\Modules\AdmPwd.PS</code> folder.

Module is compiled for use with .NET Framework 4.0. Module is however compatible with .NET Framework 2.0 as well. In case that it needs to be run on machine with only .NET Framework 2.0 installed, custom powershell.exe.config needs to be created if necessary, and needs to create the following config section:
<div class="highlight highlight-text-xml">
<pre>&lt;?<span class="pl-ent">xml</span><span class="pl-e"> version</span>=<span class="pl-s"><span class="pl-pds">"</span>1.0<span class="pl-pds">"</span></span>?&gt; 
&lt;<span class="pl-ent">configuration</span>&gt; 
    &lt;<span class="pl-ent">startup</span> <span class="pl-e">useLegacyV2RuntimeActivationPolicy</span>=<span class="pl-s"><span class="pl-pds">"</span>true<span class="pl-pds">"</span></span>&gt; 
        &lt;<span class="pl-ent">supportedRuntime</span> <span class="pl-e">version</span>=<span class="pl-s"><span class="pl-pds">"</span>v4.0.30319<span class="pl-pds">"</span></span>/&gt; 
        &lt;<span class="pl-ent">supportedRuntime</span> <span class="pl-e">version</span>=<span class="pl-s"><span class="pl-pds">"</span>v2.0.50727<span class="pl-pds">"</span></span>/&gt; 
    &lt;/<span class="pl-ent">startup</span>&gt;
&lt;/<span class="pl-ent">configuration</span>&gt;</pre>
</div>

Unattended setup of PowerShell module can be achieved by the following command line:  
<code>Msiexec /i /q AdmPwd.E.Tools.Setup.&lt;platform&gt;.msi ADDLOCAL=Management.PS</code>