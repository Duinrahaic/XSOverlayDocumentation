## Installing the SDK
!> **Requirements**
<br>- Lastest SDK from [here](https://github.com/Xiexe/XSOverlaySDK/releases/)
<br>- Unity 2019.4 (Other Unity Versions have not been tested, but might work.)
<br><br> **Installation**
<br>- Create a new Unity Project with 2019.4.x
<br>- Import the XSOverlaySDK package downloaded from above.

## Theme Creator
!> **IMPORTANT**
<br>Themes MUST use a UI shader on the material. Failing to due so may end in strange results. XSOverlay's SDK has a default UI Shader included under `XSOverlaySDK/Resources/Shaders/Default UI Template`.

> **Previewing and Building a Theme**
<br><br>- Create your material using a copy of the Default UI Template, located at `XSOverlaySDK/Resources/Shaders/Default UI Template` in your project.
<br><br> - Drag your material into the Theme Material slot on the Theme Creator component in the inspector.
<br><br> - Hit the preview button to preview your theme on the currently selected UI. (If changing Shader Parameters to get alignement correct, you may need to hit Ctrl+S to force Unity to update the material properties. This is a Unity limitation.)
<br><br> - Hit build to build your theme. You may need to hit it twice.
<br><br> - Your theme has been exported to `[ExportedThemes]/[SelectedUI]/[ThemeName]`

### Exposed Internal Shader Parameters
|Parameter                |Type     |Description                                                            |
|-------------------------|---------|-----------------------------------------------------------------------|
|`_ClockColor`            |`Color`  |If themeing the wrist, sets the clock font color to this color         |
|`_XSOAccentColor`        |`Color`  |Contains the Accent Color set in XSOverlay's user settings             |
|`_CursorPositionVelocity`|`Vector4`|Contains the Position`xy` and Velocity`zw`of the cursor on the canvas  |
|`_HMDPosition`           |`Vector3`|Contains the World Space Position of the HMD                           |
|`_HMDRotation`           |`Vector3`|Contains the World Space Rotation`eulerAngles` of the HMD              |
|`_OverlayPosition`       |`Vector3`|Contains the World Space Position of the Overlay                       |
|`_OverlayRotation`       |`Vector3`|Contains the World Space Rotation`eulerAngles` of the Overlay          |
|`_OverlayVelocity`       |`Vector3`|Contains the World Space velocity of the Overlay                       |
|`_OverlayAngleToHMD`     |`Float`  |Contains the angle from the Overlay to the HMD in degrees              |
