# Common Issues
The section below describes are some common issues that some users have encountered when using XSOverlay. 

If this does not cover your issue or need additional assistance, please create an issue on [GitHub](https://github.com/Xiexe/XSOverlay-Issue-Tracker/issues/new?assignees=&labels=bug&template=bug_report.md&title=) or Join the [Discord](https://discord.gg/cqhZpytC89) and ask for help in the #bug-reports channel.



# Troubleshooting

This covers the following issues:
	
- XSOverlay opens briefly but then closes
- XSOverlay is not displaying in game
- XSOverlay is white/not displaying information


## Administrator Rights
Previously, legacy users were required to run XSOverlay as administrator through the Windows compatibility menu for various reasons. This is no longer a requirement, as XSOverlay now has this functionality built within which ensures that things work properly. If this is your first time using XSOverlay, this is more then likely not an issue you will likely encounter.	

1. Navigate to your XSOverlay installation directory (typically in your steam folder) and find the executable `XSOverlay.exe`
2. Ensure the following options are not checked (see image below for reference):
	- Ensure that checkbox for `Run this program as an administrator` is **NOT** checked.		
    - Ensure that checkbox for `Run this program in capatibility mode for` is **NOT** checked.
3. Click `Apply` and then `OK` to save your changes.
4. Restart XSOverlay. 			
5. On next launch, a UAC prompt will appear, click `Yes` to allow XSOverlay to run as administrator. Future launches will no longer require this prompt.
6. In SteamVR, Open the XSOverlay menu, click the cog, and navigate into the settings tab. 			
7. In the first section, click 'Allow Admin Permissions'.
8. Restart XSOverlay.

![Properties](/img/XSOverlayPropertiesMarkup.png "Properties")



## Anti-Virus & Firewall
Sometimes anti-viruses detects false-positives when running XSOverlay and will prevent portions or the entirety of the application from operating. Configuration menus can differ from application to application, so refer to the manufacturer's documentation (or google) to ensure the proper steps are being applied correctly.

### Exceptions:
Add exceptions for `XSOverlay.exe`, `XSOverlay-Node-Server.exe`, and `XSOverlayMediaManager.exe` to your firewall and antivirus software, by default it uses `TCP` ports `42070` and `42071` for communication. If you are using a custom port, ensure that you add an exception for that port as well.

All executables are found in the XSOverlay installation directory, which is typically located in your steam folder.

<b>Main Executable</b>

This is the core of XSOverlay which is responsible to communicating to the other XSOverlay services:

The executable is located in the root of the installation directory and is named `XSOverlay.exe`.
This opens up `UDP` port `42069`, which is used for the Notification API.
This also connects on `TCP` port `42070` by default, which is used to communicate with the node server.


<b>Node Server</b>

This is the service that is responsible for the UI.

This is located in the `[XSOInstallDirectory]/StreamingAssets/Modules/Bridge/` directory and is named `XSOverlay-Node-Server.exe`.
This also connects on `TCP` port `42070` and `42071` by default, which is used to communicate with the node server. See `Ports Bindings` below for more information.


<b>Media Handler</b>

This is the service that is responsible for handling and managing media information and controls.

This is located in the `[XSOInstallDirectory]/StreamingAssets/Modules/Media Manager/` directory and is named `XSOverlayMediaManager.exe`.
This also connects on `TCP` port `42070` by default, which is used to communicate with the node server.



## Ports Bindings

If you are unable to bind to the default ports, you can change the ports that XSOverlay uses by doing the following:

XSOverlay, by default, is configured to listen for WebSocket messages on port `42070`. This can be changed by editing the `ExternalMessageAPIConfig.json` file located in the `[XSOverlayInstallDirectory]/XSOverlay_Data/StreamingAssets/Plugins/Config/` directory.

The following options can be configured:

| Option | Description | Default Value |
| --- | --- | --- |
| `WebSocketPort` | The port XSOverlay listens to WebSocket messages on. | `42070` |

Edit the entry for WebSocketPort to a port that you know is not taken. The app will read the configuration file and use the `WebsocketPort` and `1 + WebsocketPort`. 
For example, if set to port `40000`, the app will hook port `40000` and `40001` so make sure that both are free.

!> You will likely need to use a port number between `20000` and `49150`. It is not advised to configure XSOverlay to use ports outside of this range as it conflict with ports allocated to your operating system or subsystem services 

 

