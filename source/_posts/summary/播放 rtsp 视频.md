---
title: 播放 rtsp 视频
date: 2024-05-12 17:18:54
tags:
  - 汇总
  - rtsp
categories:
  - 汇总
---

# 1. 处理/创建 rtsp 流

## rtsp 推/拉流中转服务

### EasyDarwin - windows 版

[下载 EasyDarwin](https://github.com/EasyDarwin/EasyDarwin/releases)

```
cd EasyDarwin
./easydarwin
```

后台管理页面, 可以查看相关推流情况. [http://localhost:10008](http://localhost:10008) 默认用户/密码(admin/admin)

### mediamtx

```bash
docker run --rm -it -e MTX_PROTOCOLS=tcp -p 9554:8554 -p 1935:1935 -p 8888:8888 -p 8889:8889 -p 8890:8890/udp bluenviron/mediamtx
```

往 9554 端口推即可

```shell
rtsp://localhost:9554/{streamName}
```

## 通过 ffmpeg 进行本地推流至 rtsp 推流服务

```shell
ffmpeg -re -i 推送的视频 -rtsp_transport tcp -vcodec h264 -f rtsp rtsp://{easydarwin服务的ip}/{通道名}
```

例如:

```shell
ffmpeg -re -i /Users/4van/Downloads/test.mp4 -rtsp_transport tcp -vcodec h264 -f rtsp rtsp://localhost/test1
```
## 测试播放

[下载 VLC](https://www.videolan.org/vlc/)
打开 Network
访问: rtsp://10.211.55.4/test1
# 2. rtsp 处理服务

## rtsptoweb - 播放太卡

- 支持 rtsp 转 hls/hlsll/mse/webrtc 等格式
- 提供相应 api 创建/删除流

```bash
docker run --rm -p 8083:8083 -p 5541:5541 ghcr.io/deepch/rtsptoweb:v2.4.3
```

后台页面 [http://localhost:8083](http://localhost:8083)

## rtsp-stream - 仅支持 hls

```bash
docker run -p 80:80 -p 8080:8080 roverr/rtsp-stream:2-management
```

查看所有流 

```shell
curl --location --request GET 'http://localhost:8080/list'
```

添加流

```shell
curl --location --request POST 'http://localhost:8080/start' --header 'Content-Type: application/json' --data-raw '{ "uri": "rtsp://10.211.55.4/test1" }'
```

删除流

```shell
curl --location --request POST 'http://localhost:8080/stop' --header 'Content-Type: application/json' --data-raw '{ "id": "4f081bf2-ed1d-4881-8ff9-32f6e5532bc9", "remove": true }'
```

## rtsp2web - 仅支持 flv

原仓库 (有水印): https://github.com/Neveryu/rtsp2web
二次封装: https://github.com/Layouwen/code-example/tree/main/demos/rtsp-to-h264-flv-service

启动服务

```shell
docker build -t rtsp-to-h264-flv-service:1.0.0 .
docker run --rm -p 9999:4384 -e AUDIO=true rtsp-to-h264-flv-service:1.0.0
```

前端直接传 rtsp 地址进行播放

```tsx
export const Jsmpeg: FC<{ baseUrl: string; rtsp: string }> = ({ baseUrl, rtsp }) => {
  const videoPlayerRef = useRef<any>(null)

  const onPlay = () => {
    const VideoPlayer = (window as any).JSMpeg

    videoPlayerRef.current = new VideoPlayer.Player(
      `ws://localhost:9999/rtsp?url=` +
      btoa(rtsp) + '&brightness=0.2&saturation=1.8',
      {
        canvas: document.getElementById('video-player'),
      }
    )
  }

  return (
    <div>
      <div>
        <button onClick={onPlay}>播放</button>
      </div>
      <canvas id="video-player" />
    </div>
  )
}
```

## zlmediakit

```shell
docker run --rm -p 2935:1935 -p 8080:80 -p 8443:443 -p 8554:554 -p 10000:10000 -p 10000:10000/udp -p 8000:8000/udp -p 9000:9000/udp zlmediakit/zlmediakit:master
```

示例 url:
- http://127.0.0.1:8080/live/test1.live.flv?vhost=192.168.10.11
- ws://127.0.0.1:8080/live/test1.live.flv?vhost=192.168.10.11
- https://tdev1.locnavi.com/live/test1.live.flv?vhost=192.168.10.11

# 3. 相关资料

rtsp h.264 to h.265

```bash
ffmpeg -i {rtsp转换前地址} -c:v libx265 -c:a copy -f rtsp {rtsp转换后地址}
```

mp4 to rtsp h264

```bash
ffmpeg -i {视频文件} -c:v libx264 -c:a copy -f rtsp {rtsp转换后地址}
```

mp4 to rtsp h265

```bash
ffmpeg -i {视频文件} -c:v libx265 -c:a copy -f rtsp {rtsp转换后地址}
```

hls 测试地址

[https:/bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8](https:/bitdash-a.akamaihd.net/content/sintel/hls/playlist.m3u8)

# 4. 其他库

## 播放器

- h265
	- https://github.com/numberwolf/h265web.js
	- https://github.com/langhuihui/jessibuca
- hls 播放库
	- https://github.com/foxford/react-hls
