# Windows Application Package Manifest
To help developers build Windows application packages that end-users love, enterprises want, and Windows OS needs.

## Why you should follow this manifest?

1. **Lower security risk by building Windows applications and their installation packages based on Microsoft security standards.**
2. **Increase application sales and user base.** Build your application in a way enterprises don't need to spend money and time to repackage it to match the industry standards. There is a higher chance for enterprises to choose applications that follow this manifest file.
3. **Improve user expriecne.** Make the application user-friendly.
4. **Easier app maintenance for developers.** Understanding common practices and the why behind them, can significantly lower the rate of major architectural changes for the application in the future.


## General
 - **Application must be able to install and uninstall silently using system account**
   	 - Without the need for user input
   	 - Without visible popups
   	 - Without opening other applications
   	 - Without the need to restart the machine
 - **Everything that is installed must be uninstalled**
   	 - Exception is configuration and user data can be left installed

## Security
**Application data** - data that is created by the application.  
**User data** - application data created by a specific user and belonging to a specific user.  

It's important to install and allow binary creation in specific Windows OS locations to ensure maximum security, packaging format compatibility, and application ease of use.

### Files
  - 64bit app → “C:\Program Files\<Company Name>\<App Name>"
  - 32bit app → “C:\Program Files (x86))\<Company Name>\<App Name>"

### Code signing
All application binaries must be signed
  - All executable files (.exe, .dll, .ocx, .sys, .cpl, .drv, .scr, etc.) must be signed
  - All used scripts (.ps1, vbs, vba, bat, .js, .jse, etc.)
  - Sign your package with the same certificate you sign application binaries

### Data
  - Shared with all users → “C:\ProgramData\<App Name>"
  - Per-user
     - Roaming data → “C:\Users\<username>\AppData\Roaming\<App Name>"
     - Not roaming (medium integrity data) → “C:\Users\<username>\AppData\Local\<App Name>"
     - Not roaming (low integrity data) → “C:\Users\<username>\AppData\Local\Low\<App Name>"

### Registries
 - Per-machine → HKEY_LOCAL_MACHINE\Software\<App Name>
 - Per-user → HKEY_CURRENT_USER\Software\<App Name>
 - If the app requires changes for system boot → HKEY_LOCAL_MACHINE\System
 - Do not create or modify any other registry key.

### Permissions
"In Windows, there is no security if you run as admin." /Sami Laiho
  - Remove the executable need for admin rights → Develop device driver instead
  - If MSIX and Windows 11
       - [App Isolation](https://github.com/microsoft/win32-app-isolation/tree/main)

## Package
 - Build your installer package in the **MSIX** or **MSI** packaging formats. If can, choose MSIX as the primer packaging type.
 - Avoid EXE and other formats as they can slow down the enterprise application management lifecycle.

### MSI
 - Do not use VbSCript

### MSIX
 - Be aware of MSIX packaging type limitations and benefits.

### Configuration
 - Make it possible to configure your applications via the command line during installation
     - That is needed so that enterprises do need to repackage the application

### Installation wizard
 - Do not build one. Have a simple as possible way to install and uninstall the app.
 - Do not allow users to change application settings during the installation. Instead, build the app configuration part within the app AND have command line options for admins to pre-configure the app for the end-users.
 - Selecting specific features of the app
     - Install all features by default and allow them to be configured within the app or installation command line.
     - If the specific feature is larger than 1GB consider building a separate installer package as an add-on for your app.
  
## User Experience
Following this can improve user experience when interacting with Windows applications.
Make the end-user environment clean and understandable.

### Application Name
  - Application Name without version
     - _Example: "App Name"_

### Application Version
  - Major.Minor.Build.Revision
     - Major cannot be 0 for MSIX packages.
     - Revision can be skipped for MSI packages.
 
### Installation Directory folder structure
  - Company Name\App Name
  - _Example: "C:\Program Files (x86)\Vendor\App Name"_

### Shortcuts
 - Default location is → "C:\ProgramData\Microsoft\Windows\Start Menu\Programs"
    - If the application has more than one shortcut create a folder to combine them
    - _Example: "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\App Name"_
 - Name the shortcut without the app version
      - _Example: "App Name"_
 - Do not create desktop and taskbar shortcuts. Desktop and taskbar is end-user private space and they can create shortcuts there on their own.
 - Do not create uninstall shortcuts. Windows offers an uninstall option by right-clicking on any app shortcut and from Programs and Features.
 - Do not create rarely-needed shortcuts like links to the website, release notes, etc.

### Office 365 add-ins
 - New Office versions will support only Web add-ins. Microsoft has no plans to continue to support COM/VSTO Add-ins.

## References
- [Certification Requirements for Windows Desktop Apps](https://learn.microsoft.com/en-us/windows/win32/win_cert/certification-requirements-for-windows-desktop-apps)
- [NCSC End User Device Guidance](https://www.ncsc.gov.uk/collection/device-security-guidance/platform-guides/windows)
- [How to Build Better and More Secure Software for Windows by Sami Laiho](https://youtu.be/-xk9lQf27wM)
- [Reddit EXE vs MSI : r/sysadmin ([https://youtu.be/-xk9lQf27wM](https://www.reddit.com/r/sysadmin/comments/1473cq4/exe_vs_msi/)
- [Smart App Control](https://learn.microsoft.com/en-us/windows/apps/develop/smart-app-control/overview)
- [Microsoft Office 365 Add-ins](https://techcommunity.microsoft.com/t5/outlook-blog/things-to-know-about-the-new-outlook-for-windows/ba-p/3383964)
- [Deprecated features for Windows client](https://learn.microsoft.com/en-us/windows/whats-new/deprecated-features)
- [MSIX Know Your Installer](https://learn.microsoft.com/en-us/windows/msix/packaging-tool/know-your-installer)
