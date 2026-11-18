Installing Custom Apps on Windows and Mac Devices
One of the most powerful features of Systems Manager is its ability to remotely install and uninstall applications from your devices. The Software Installer's flexibility allows for one-click deployment of applications across all of your devices, regardless of operating system.
To see information on how to deploy scripts to Windows and Mac devices, see this article.
This feature is not available for Legacy Systems Manager users.
Prerequisite: Installing applications on Mac and Windows devices requires that the agent be installed. Read more about agent installation here.
Note: macOS devices can install apps using the device's native Mac App Store in conjunction with Apple Volume Purchase Program via your Apple School Manager or Apple Business Manager account and Meraki Systems Manager. To do this, enroll the macOS device with the Meraki Management enrollment profile. Detailed steps for Mac App Store app deployments within Systems Manager can be found here.
Installing Applications on Devices
Using Systems Manager, suites of applications can very easily be deployed to end user devices.
The following instructions outline how to deploy a new application, as well as overview additional installation options:
- Navigate to Systems Manager > Manage > Apps
- Click on the + Add app button and select an appropriate App platform:
 
- Once an App platform Custom app is selected, fill in the following information about the application:
- Name (required): The name of the application as it will appear on the end device (e.g. Firefox, FileZilla, etc). If the application is already installed on managed clients, it will be listed in the drop-down menu. This name can be changed at any time.
The application name must exactly match the application name that is detected by the end device.
- Identifier (optional): The vendor specified app identifier.
- Icon URL (optional): A URL containing the app icon which will be used for the custom app.
- Vendor (optional): The vendor of the specified application.
- Version (optional): The version number of the application to be installed.
Note: If no version is entered, Dashboard defaults to "0", and the version is then required when editing the app.
- Description (optional): A brief description of the application.
- Source (required): This option denotes whether the application's installer will be hosted in Dashboard, or on an external server (e.g. Dropbox).
    
  - Upload to the Meraki Cloud: Select this option and click Browse to upload the installer file directly to Dashboard.
  - Specify a URL: Select this option if the installer file is hosted on an external server or file share site, and specify the URL for the hosted file. The URL field must point to the direct download link to a publicly hosted file or an internally hosted server accessible by the end user's device.
Note: The installer must be silent (require no user interaction) in order for the application to install correctly in the background. Windows applications can be installed in the foreground in order to prompt user interaction.
Note: The following installer file types are supported:
Windows: .exe, .msi
macOS:  .dmg, .pkg
Note: macOS may install an .app but it must be encapsulated inside a .dmg image or within a .pkg package. For more information, review Apple's Developer Documentation.
- Keep app up to date: Specifies if the app can automatically keep app up to date with the latest version available. When a targeted device checks in and an app update is available, an app upgrade request will be automatically sent to the device.
- Auto-install: By default, the application will be auto-installed the moment the Save button is clicked. If auto-install is disabled, the application won't be installed until it's manually pushed by navigating to Systems manager > Manage > Apps, and selecting Push > Push to all. Dashboard users can manually queue install requests via the device page, or by pressing the Repush buttons on this page.
- Install in foreground (Windows-only): Allow the installer to show user prompts, instead of silently installing in the background.
- Installation arguments (optional): If the installer must be run with specific command line arguments, they can be listed here.
 The following list shows the actual install command run on the end-device, where [arguments] refers to the contents of the Installation arguments field in Dashboard:
  - EXE: application_installer.exe [arguments]
  - MSI: msiexec /quiet /i application_installer.msi [arguments]
  - PKG: /usr/sbin/installer [arguments] -pkg application_installer.pkg -target /
- Command line (optional): Specifies a command to run after installation has completed. This is commonly used to reboot the machine after installation, using the following commands:
    
  - Windows: shutdown /r
  - OS X: shutdown -r now
- Visible in SSP: Specifies if the app can be added or removed by end users from the Self Service Portal (SSP).
- Targets: Specifies a scope of devices that will have this application installed. Please refer to our documentation for more information on scoping by device tag.
4. Click Save Changes. Unless auto-install is disabled, this will push out the application to all devices within the specified scope.
Example Configuration
The following example configuration will quietly install FileZilla from a Dropbox URL on all managed Windows clients with the "bginstall" tag. These clients will also reboot after installation has completed.
Monitoring Installation
The status of an installation can be checked via the Monitor > Devices page, by clicking on a particular client and navigating to the Event log or Activity log section.
Removing Applications from Devices for Windows
Certain .msi and .app programs can be remotely uninstalled via the Systems Manager > Monitor > Software page. To remove a specific application, search for the application under Software inventory and click uninstall.
Note: That software inventory "uninstall" button for windows agent installed apps only works if there is an uninstaller file for the application in "c:\windows\program files\". If this is not the case, create your own .msi or .exe that does the uninstall, upload that as a custom app to Dashboard and scope it for the specific windows device.
Note: Auto-uninstall for an app pushed from the dashboard on windows devices doesn't automatically remove an app when that app is removed from the device's scope.
Removing Applications from Devices for MacOS
At this time, the Systems Manager agent does not support uninstallation of custom apps on MacOS.
Troubleshooting
If the software install command is issued and the application does not appear on the client device, check the Dashboard-side logging to see if the app install command errored out. If you see a 'Success' entry, this means the command successfully went through, but may not have completely executed on the client device. At this point, you should reference the agent logs on the device itself, and provide these to Meraki Support for troubleshooting assistance.
App is installed on device but reports as Missing or Not Installed
This can happen for several reasons, but it is generally a mismatch between the actual installed app name and the manually configured SM > Apps name. If the App name on SM > Apps is different than the actual name of the installed app reported to Systems Manager from the device the app will report as "Not Installed". To troubleshoot this, look at the actual name of the app according to the device. Click on the device and find the app in the installed App List:
The name, "Google Chrome", reported from the device must be the same name we give for the app on SM > Apps:
Other related issues:
- App must be installed from SM > App custom app as "Success" at least once.
- Multiple of the same App Name on the SM > Apps page is not supported. If multiple of the same app name are configured on SM > Apps, they may both report as "Not Installed".
- If the app is never reported as an "Installed" on the device, it will always show as "Not Installed" and missing this app.
    
  - For macOS, the app is reported as "installed" if the device sees it installed locally in:
        
    - About This Mac > System Report... > Application
  - The SM Agent collects the above information with the following terminal command:
        
    - system_profiler -xml SPApplicationsDataType
- For macOS, the app is reported as "installed" if the device sees it installed locally in: