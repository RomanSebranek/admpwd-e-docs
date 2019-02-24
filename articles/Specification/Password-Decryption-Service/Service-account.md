# Service account

Default service account for PDS is `NETWORK SERVICE`. In this configuration service uses SPN `HOST/computername`

It is however supported to run the service under domain account. To do so service SPN must be changed and registered with domain account. Change must be performed on both service side (all running instances) and client side.

**Important** All instances of PDS must use the same service account - either NETWORK SERVICE or domain account. Mixing of service accounts is not supported.

To change service identity on server side, change it in AdmPwd.PDS.exe.config from <code>dns</code> to <code>servicePrincipalName</code> and set its value to <code>SVC/AdmPwd</code> as shown below:

<div class="highlight highlight-text-xml">
<pre>&lt;<span class="pl-ent">service</span> <span class="pl-e">behaviorConfiguration</span>=<span class="pl-s"><span class="pl-pds">"</span>AdmPwdService.ServiceBehavior<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>AdmPwd.PDS.AdmPwdSvc<span class="pl-pds">"</span></span>&gt;
  &lt;<span class="pl-ent">endpoint</span> <span class="pl-e">address</span>=<span class="pl-s"><span class="pl-pds">"</span><span class="pl-pds">"</span></span> <span class="pl-e">binding</span>=<span class="pl-s"><span class="pl-pds">"</span>netTcpBinding<span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>NetTcpEndpoint<span class="pl-pds">"</span></span> <span class="pl-e">contract</span>=<span class="pl-s"><span class="pl-pds">"</span>AdmPwd.PDS.IAdmPwdSvc<span class="pl-pds">"</span></span>&gt;
    &lt;<span class="pl-ent">identity</span>&gt;
      &lt;<strong><span class="pl-ent">servicePrincipalName</span> value = <span class="pl-s"><span class="pl-pds">"</span>SVC/AdmPwd<span class="pl-pds">"</span></span></strong> /&gt;
    &lt;/<span class="pl-ent">identity</span>&gt;
  &lt;/<span class="pl-ent">endpoint</span>&gt;
  &lt;<span class="pl-ent">endpoint</span> <span class="pl-e">address</span>=<span class="pl-s"><span class="pl-pds">"</span>mex<span class="pl-pds">"</span></span> <span class="pl-e">binding</span>=<span class="pl-s"><span class="pl-pds">"</span>mexTcpBinding<span class="pl-pds">"</span></span> <span class="pl-e">bindingConfiguration</span>=<span class="pl-s"><span class="pl-pds">"</span><span class="pl-pds">"</span></span> <span class="pl-e">name</span>=<span class="pl-s"><span class="pl-pds">"</span>MexTcpEndpoint<span class="pl-pds">"</span></span> <span class="pl-e">contract</span>=<span class="pl-s"><span class="pl-pds">"</span>IMetadataExchange<span class="pl-pds">"</span></span> /&gt;
  &lt;<span class="pl-ent">host</span>&gt;
    &lt;<span class="pl-ent">baseAddresses</span>&gt;
      &lt;<span class="pl-ent">add</span> <span class="pl-e">baseAddress</span>=<span class="pl-s"><span class="pl-pds">"</span>net.tcp://localhost:61184/AdmPwdService<span class="pl-pds">"</span></span> /&gt;
    &lt;/<span class="pl-ent">baseAddresses</span>&gt;
  &lt;/<span class="pl-ent">host</span>&gt;
&lt;/<span class="pl-ent">service</span>&gt;</pre>
</div>

**Important**: content of the config file is case sensitive. Please make sure you use the case as shown in sample above when making changes

Change of service account on server side must be supported by configuration of management tools side as well. In default configuration, management tools expect that service uses SPN `HOST/<computername>`. In configuration with domain account, management tools need to use service specific SPN `SVC/AdmPwd` when calling PDS.

This change of configuration of management tools can be done via registry using GPO as specified in [Management tools configuration](../Management-Tools/Configuration.md)

Checklist for changing from `NETWORK SERVICE` account to domain account:
1. Create account for service (service account) in domain
2. Register SPN SVC/AdmPwd on service account
3. Grant service account Read permission on PDS install folder (`%ProgramFiles64%\AdmPwd\PDS`) and Modify permission to CryptoKeyStorage folder (`%ProgramFiles64%\AdmPwd\PDS\CryptoKeyStorage` by default)
4. Grant service account PDS permissions on AD via respective PowerShell cmdlets:
   * `Set-AdmPwdPdsPermission`, 
   * `Set-AdmPwdPdsDeletedObjectsPermission` 
   * `Set-AdmPwdPdsManagedAccountsPermission`
5. Grant service account permission to read/write SRV record in DNS; or turn off SRV record registration and maintain record manually
6. Configure Group Policy “PDS service runs using domain account” to Enabled and apply it to machines that are running management tools. This includes all machines that are running at least one of following:
    * PowerShell module
 	* Fat client UI
 	* Web UI
7. Set domain account as logon identity of PDS Win32 service and restart the service
