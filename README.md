master is not a stable branch, you may want to see the latest [tag](https://github.com/a1q123456/rtmp-sharp-server/tree/v0.0.1)

# Harmonic
A high performance RTMP live streaming application framework


# Getting started

## Code

Program.cs

```csharp
using Harmonic.Hosting;
using System;
using System.Net;

namespace demo
{
    class Program
    {
        static void Main(string[] args)
        {
            RtmpServer server = new RtmpServerBuilder()
                .UseStartup<Startup>()
                .Build();
            var tsk = server.StartAsync();
            tsk.Wait();
        }
    }
}

```

StartUp.cs
```csharp
using Autofac;
using Harmonic.Hosting;

namespace demo
{
    class Startup : IStartup
    {
        public void ConfigureServices(ContainerBuilder builder)
        {

        }
    }
}

```

Build a server like this to support websocket-flv transmission

```csharp
RtmpServer server = new RtmpServerBuilder()
    .UseStartup<Startup>()
    .UseWebSocket(c =>
    {
        c.BindEndPoint = new IPEndPoint(IPAddress.Parse("0.0.0.0"), 8080);
    })
    .Build();

```

## push video file using ffmpeg
```bash
ffmpeg -i test.mp4 -f flv -vcodec h264 -acodec aac "rtmp://127.0.0.1/living/streamName"
```
## play rtmp stream using ffplay

```bash
ffplay "rtmp://127.0.0.1/living/streamName"
```

## play flv stream using [flv.js](https://github.com/Bilibili/flv.js) by websocket

```html
<video id="player"></video>

<script>

    if (flvjs.isSupported()) {
        var player = document.getElementById('player');
        var flvPlayer = flvjs.createPlayer({
            type: 'flv',
            url: "ws://127.0.0.1/websocketplay/streamName"
        });
        flvPlayer.attachMediaElement(player);
        flvPlayer.load();
        flvPlayer.play();
    }
</script>
```


# Dive in deep
You can view docs [here](docs/README.md)

