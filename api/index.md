# Introduction
This help file covers interface provided by AdmPwd Enterprise PDS integration library. Purpose of the integration library is to allow applications easily integrate with solution functionality:
* Allow users to read and reset passwords of managed accounts (local admin accounts or managed domain user accounts)
* Generate key pairs
* Use other functionality provided by Password Decryption Service (PDS)</para>
* Get information about state of environment, including licensing details

All this without the need to know details about how to:
* Discover online instance of PDS
* Connect to PDS interface and perform authentication

# Getting Started
To get started, add a reference to integration library to your project, and call static methods exposed by the library. Methods are pretty simple to use and map to high level functionality of the solution.

For example, reading a password of managed admin account is just single line of code in your application.
For code examples, see open source project with code samples on <a href="https://github.com/jformacek/admpwd-e/tree/master/Clients" target="_blank">GitHub</a>

Enter the reference docs pages [**HERE**](../articles/Wrapper-Intro.md) 