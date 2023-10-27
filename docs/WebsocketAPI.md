# XSOverlay Websocket API

XSOverlay has a websocket based API for interacting with the XSOverlay media player and notification interfaces. 

!>_This is API is currently in development and may not be fully implemented yet. Please visit the discord for further updates._

***
# Configuration

XSOverlay, by default, is configured to listen for websocket messages on port `42070`. This can be changed by editing the `ExternalMessageAPIConfig.json` file located in the `[XSOverlayInstallDirectory]/XSOverlay_Data/StreamingAssets/Plugins/Config/` directory.

The following options can be configured:

| Option | Description | Default Value |
| --- | --- | --- |
| `WebSocketPort` | The port XSOverlay listens to websocket messages on. | `42070` |


!>_XSOverlay **ONLY** listens for messages from the local host. You **CANNOT** currently send messages over the network to a different machine_

***

# Websocket Message Format

All websocket messages are sent as JSON objects. The following properties are available:

| Property | Data Type | Description |
| --- | --- | --- |
| sender | string | The name of the application sending the message |
| target | string | The name of the application the message is intended for. Ex 'xsoverlay'|
| command | string | The command to issue to the target |
| jsonData | string | JSON Payload with command specific data. See below for endpoint specific for endpoint message formats |
| rawData | string | Raw data payload with a signular value in string format like "true", "false", etc. This is mainly used by the UI to do things like setting settings, where the string will be parsed|


# Code Snippet 
A C# example of connecting to the XSOverlay websocket server. This is a C# example, but the same concept can be applied to any language that supports websockets and JSON serialization.
```cs
static async Task Main(string[] args)
{
      WebsocketPort = (args.Length == 0) ? 42070 : Int32.Parse(args[0]);
      var exitEvent = new ManualResetEvent(false);
      var wsURL = new Uri($"ws://localhost:{WebsocketPort}/?client=[YOURAPPNAMEHERE]");
      // The format of this URL is very important. Client token needs to be there and should be the name of your application, if a client token is not present, the server will reject the connection.

      // If a client token is already in use, the server will simply add it to a list. This means that if anything sends a message to your client token, it will be sent to everything in that list for the token.
      
      using (Socket = new WebsocketClient(wsURL))
      {
          Socket.Start();
      }
}
```



# Media Player

The media API allows you to control the XSOverlay media player.

## Message Format

| Property | Data Type | Description |
| --- | --- | --- |
| Title | string | Media Title |
| Album | string | Media Album |
| Artist | string | Media Artist |
| AlbumArt | string | Media Album Art in Base64 |
| ApplicationName | string |  App Name (Spotify, Apple Music, Groove, etc) |
| PlaybackStatus | string | Playback status (Playing, Paused, etc) this will just control the play / pause icon, purely visual. |


## Code Snippet 
An example for the updating the endpoint with a media player update. This is a C# example, but the same concept can be applied to any language that supports websockets and JSON serialization.
```cs 

public static void SendXSOMessage()
{
    XSOMessage msg = new XSOMessage();
    msg.sender = "XSOverlayMediaManager"; // Should just be whatever you want to call your application
    msg.target = "xsoverlay"; // Also important, this tells the node bridge that you are targeting the XSOverlay client.
    msg.command = "UpdateMediaPlayerInformation"; // This is important, and case sensitive, it is the command that XSO will use to know what to do with the data
    msg.jsonData = JsonSerializer.Serialize(CurrentMedia); // Current media object is an XSOMediaObject in this case.

    if (!Socket.IsRunning)
        return;

    string newMediaUpdate = JsonSerializer.Serialize(msg);
    if (LastMediaUpdate != newMediaUpdate)
    {
        Task.Run(() => Socket.Send(newMediaUpdate));
        LastMediaUpdate = newMediaUpdate;
    }
}


```


# Notifications

The Notification Endpoint allows you to send notifications to the user's overlay.

![Notification Markup](/img/notification/NotificationMarkup.png "Notification Markup")

## Message Format



## Code Snippet 
