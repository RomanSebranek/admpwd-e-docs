# Fat client UI
Fat cient UI is installed to <code>%ProgramFiles%\AdmPwd\UI</code>.

Unattended setup of Fat client can be achieved by the following command line:  
<code>Msiexec /i /q AdmPwd.E.Tools.Setup.&lt;platform&gt;.msi ADDLOCAL=Management.UI</code>

Fat client supports running from network share; to do so, all files contained in its install folder (without subfolders) need to be copied to network share.

Fat client also supports to be run as a right-click context menu extension for computer objects in Active Directory Users and Computers (dsa.msc).  
*Note*: for configuration details, see [here](http://etutorials.org/Server+Administration/Active+directory/Part+III+Scripting+Active+Directory+with+ADSI+ADO+and+WMI/Chapter+24.+Extending+the+Schema+and+the+Active+Directory+Snap-Ins/24.2+Customizing+the+Active+Directory+Administrative+Snap-ins/)