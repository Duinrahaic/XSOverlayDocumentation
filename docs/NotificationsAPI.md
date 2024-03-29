# Notifications API

XSOverlay has a websocket based API for sending notifications to the overlay. 

!> **Notice:** XSOverlay is migrating the Notification API to a websocket protocol. This API remain for legacy application, but futher development and support will be migrated to [Websocket API](WebsocketAPI)


***
# Configuration

XSOverlay, by default, is configured to listen for Notifications messages on port `42069`. This can be changed by editing the `ExternalMessageAPIConfig.json` file located in the `[XSOverlayInstallDirectory]/XSOverlay_Data/StreamingAssets/Plugins/Config/` directory.

The following options can be configured:

| Option | Description | Default Value |
| --- | --- | --- |
| `UdpPort` | The UDP port XSOverlay listens to messages on. | `42069` |


!>_XSOverlay **ONLY** listens for messages from the local host. You **CANNOT** currently send messages over the network to a different machine_

***
# XSOverlay Message Object

The following is the message object that is to be sent to XSOverlay Notification API.

![Notification Markup](/img/notification/NotificationMarkup.png "Notification Markup")


| Prorperty | Type | Description | Default Value |
| --- | --- | --- | --- |
| `messageType` | `int` | The type of message to send. `1` defines that this is for the Notification API | `1` |
| `index` | `int` | Used for Media Player, changes the icon on the wrist. (depricated, see note below) | `0` |
| `timeout` | `float` | How long the notification will stay on screen for in seconds. | `0.5f` |
| `height` | `float` | Height notification will expand to if it has content other than a title. Default is 175. | `175` |
| `opacity` | `float` | Opacity of the notification, to make it less intrusive. Setting to 0 will set to 1. | `1` |
| `volume` | `float` | Notification sound volume. | `0.7f` |
| `audioPath` | `string` | File path to .ogg audio file. Can be "default", "error", or "warning". Notification will be silent if left empty. | `""` |
| `title` | `string` | Notification title, supports Rich Text Formatting. | `""` |
| `content` | `string` | Notification content, supports Rich Text Formatting, if left empty, notification will be small. | `""` |
| `useBase64Icon` | `bool` | Set to true if using Base64 for the icon image. | `false` |
| `icon` | `string` | Base64 Encoded image, or file path to image. Can also be "default", "error", or "warning". | `""` |
| `sourceApp` | `string` | Somewhere to put your app name for debugging purposes. | `""` |

!>_Media related properties have been depricated_


***
# C# Nuget Package
XSOverlay has an officially maintained [NuGet Package](https://www.nuget.org/packages/XSNotifications/) / [Github repository](https://github.com/nnaaa-vr/XSNotifications) for integrating the Notifications API easily into your C# applications.

## Code Samples
```cs
//...
using XSNotifications;
using XSNotifications.Enum;

namespace NotificationExample1
{
    class Program
    {
        static void Main(string[] args)
        {
            new XSNotifier().SendNotification(new XSNotification(){Title="Notification"});
        }
    }
}
```


***
## C# Manual Integration
```cs
class Program
{
    private struct XSOMessage
    {
        public int messageType { get; set; }
        public int index { get; set; }
        public float volume { get; set; }
        public string audioPath { get; set; }
        public float timeout { get; set; }
        public string title { get; set; }
        public string content { get; set; }
        public string icon { get; set; }
        public float height { get; set; }
        public float opacity { get; set; }
        public bool useBase64Icon { get; set; }
        public string sourceApp { get; set; }
    }

    private const int Port = 42069;

    private static void Main()
    {
        IPAddress broadcastIP = IPAddress.Parse("127.0.0.1");
        Socket broadcastSocket = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);
        IPEndPoint endPoint = new IPEndPoint(broadcastIP, Port);

        XSOMessage msg = new XSOMessage();
        msg.messageType = 1;
        msg.title = "Example Notification!";
        msg.content = "It's an example!";
        msg.height = 120f;
        msg.sourceApp = "XSOverlay_Example_UDP";
        msg.timeout = 6;
        msg.volume = 0.5f;
        msg.audioPath = "default";
        msg.useBase64Icon = false;
        msg.icon = "default";
        msg.opacity = 1f;

        byte[] byteBuffer = JsonSerializer.SerializeToUtf8Bytes(msg);
        broadcastSocket.SendTo(byteBuffer, endPoint);
    }
}
```