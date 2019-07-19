# Introduction

This reference covers interface provided by AdmPwd Enterprise PDS integration library (aka 'PDS Wrapper'). Purpose of the integration library is to allow applications easily integrate with solution functionality:
* Allow users to read and reset passwords of managed accounts (local admin accounts or managed domain user accounts)
* Generate encryption/decryption key pairs
* Use other functionality provided by Password Decryption Service (PDS)
* Get information about state of environment

All this without the need to know details about how to:
* Discover online instance of PDS
* Connect to PDS interface and perform authentication


## Getting Started
To get started, add a reference to integration library to your project, and call static methods exposed by the library. Methods are pretty simple to use and map to high level functionality of the solution.

Library is provided as NuGet package [`Greycorbel.AdmPwd-E.PDSWrapper`](https://www.nuget.org/packages/Greycorbel.AdmPwd-E.PDSWrapper/)

For more information, see [Integration SDK](Specification/Management-Tools.md#integration-sdk)

## Version history

|Version|Released|
|-------|--------|
|[v7.5.0.2](Version/v7.5.0.2.md)|28 Nov 2016|
|[v7.5.1.0](Version/v7.5.1.0.md)|22 Jan 2017|
|[v7.5.2.0](Version/v7.5.2.0.md)|27 Mar 2017|
|[v7.5.3.0](Version/v7.5.3.0.md)|09 Sep 2017|
|[v7.5.4.0](Version/v7.5.4.0.md)|27 Dec 2017|
|[v7.5.5.0](Version/v7.5.5.0.md)|01 May 2018|
|[v7.6.0.0](Version/v7.6.0.0.md)|31 Dec 2018|
|[v7.7.0.0](Version/v7.7.0.0.md)|14 Jul 2019|