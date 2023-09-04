# Windows Application Package Manifest
To help developers build Windows application packages that end-users love, enterprises want, and Windows OS needs.

## General

 - **Application must be able to install and uninstall silently**

   	 - Without the need for user input
   	 - Without visible popups
   	 - Without opening other applications
   	  
 - **Everything that is installed must be uninstalled**

   	 - Exception is configuration and user data can be left installed
  
 - **All application binaries must be signed**

  	 - All executable files (.exe, .dll, .ocx, .sys, .cpl, .drv, .scr) must be signed
   	 - All used scripts (.ps1, vbs, bat, etc.)
   	 - Sign your package with the same certificate you sign application binaries

## Binaries
**Application data** - data that is created by the application.  
**Configuration data** - data that holds configuration.  
**User data** - application-created data created by a specific user and belonging to a specific user.  

It's important to install and allow binary creation in specific Windows OS locations to ensure maximum security, packaging format compatibility, and application ease of use. 

### Files
  - 64bit app → “C:\Program Files\<Company Name>\<App Name>"
  - 32bit app → “C:\Program Files (x86))\<Company Name>\<App Name>"


