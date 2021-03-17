# Notifications API

!>_XSOverlay's default UDP port is `42069`. This can be changed by navigating to `[XSOverlayInstallDirectory]/XSOverlay_Data/StreamingAssets/Etc/` and changing the port in the `NotificationAPIConfig.json` file._

!>_XSOverlay **ONLY** listens for messages from localhost. You **CANNOT** currently send messages over the network to a different machine._

***
# XSOverlay Message Object
```cs
public int messageType = 0; // 1 = Notification Popup, 2 = MediaPlayer Information, will be extended later on.
public int index = 0; //Only used for Media Player, changes the icon on the wrist.
public float timeout = 0.5f; //How long the notification will stay on screen for in seconds
public float height = 175; //Height notification will expand to if it has content other than a title. Default is 175
public float opacity = 1; //Opacity of the notification, to make it less intrusive. Setting to 0 will set to 1.
public float volume = 0.7f; // Notification sound volume.
public string audioPath = ""; //File path to .ogg audio file. Can be "default", "error", or "warning". Notification will be silent if left empty.
public string title = ""; //Notification title, supports Rich Text Formatting
public string content = ""; //Notification content, supports Rich Text Formatting, if left empty, notification will be small.
public bool useBase64Icon = false; //Set to true if using Base64 for the icon image
public string icon = ""; //Base64 Encoded image, or file path to image. Can also be "default", "error", or "warning"
public string sourceApp = ""; //Somewhere to put your app name for debugging purposes

```

***
# Simple Integration Example in C#
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