# Management Tools
Management tools coming with solution include:
* Powershell module
* Fat UI client

There are more management tools available on [GitHub](https://github.com/GreyCorbel/admpwd-e/tree/master/Clients).

## Integration SDK
It's very easy to create own tools that make use of AdmPwd.E solution - this is because of [Integration SDK](../Wrapper-Intro.md) that provides wrapper around all heavy lifting related to service discovery and tools configuration (it processes all GPO settings used to configure the tools), and provides high level API to call into. All management tools we deliver make use of this integration SDK.

SDK is available as NuGet package [here](https://www.nuget.org/packages/Greycorbel.AdmPwd-E.PDSWrapper/). You can install it via the command: `Install-Package Greycorbel.AdmPwd-E.PDSWrapper`

Sections below describe:
* [Configuration settings for management tools](./Management-Tools/Configuration.md)
* [Commands implemented by PowerShell module](./Management-Tools/PowerShell-Module.md)
