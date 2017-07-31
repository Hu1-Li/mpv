# mpv_tv
使用mpv看直播和番剧

事实上所有支持HLS协议的软件都可以，比如Safari, Chrome, QuickTime Player.

## Install First
`mpv`

`youtube-dl`

## Bilibili Live
B站的H5直播在界面上有直接切换的按钮，因此只需要在页面上切换，然后使用调试模式获取直播源地址。

实际在浏览器上发生的请求是：

```
curl 'https://api.live.bilibili.com/api/playurl?device=phone&platform=ios&scale=3&build=10000&cid=280446&otype=json&platform=h5' \
-XGET \
-H 'DNT: 1' \
-H 'Referer: https://live.bilibili.com/h5/387' \
-H 'Origin: https://live.bilibili.com' \
-H 'Accept: application/json, text/plain, */*' \
-H 'User-Agent: Mozilla/5.0 (iPad; CPU OS 10_0 like Mac OS X) AppleWebKit/602.1.38 (KHTML, like Gecko) Version/10.0 Mobile/14A300 Safari/602.1'
```

其响应结果是：

```
{
    "code": 0,
    "msg": "",
    "current_quality": 4,
    "data": "http://xl.live-play.acgvideo.com/live-xl/126075/live_8192168_5923385.m3u8?wsSecret=2b8696b694d9ebb517ebe08ff6be7810&wsTime=1501121334"
}
```

这里请求的cid就是房间号.

这里只需要这个`data`字段，然后使用`mpv http://xl.live-play.acgvideo.com/live-xl/126075/live_8192168_5923385.m3u8?wsSecret=2b8696b694d9ebb517ebe08ff6be7810&wsTime=1501121334` 即可观看直播。


## PandaTV Live
熊猫直播可以使用浏览器的切换UA的方法获取相应的H5直播地址（使用调试模式，选取视频区域就可以看到直播地址）

然后使用`mpv https://pl-hls21.live.panda.tv/live_panda/d09ed7f66d35bbe66c211a68b6e6cf84.m3u8`就可以观看直播了。


## Douyu Live
斗鱼直播切换UA之后获取的是标清资源，目前还不知道怎么获取超清直播地址。

## Bilibili TV
看B站视频的话可以直接使用 `mpv https://www.bilibili.com/video/av12528578/`即可。

# Problem
实际上mpv看直播有一个问题：如果网卡或者其他原因导致mpv buffer，之后的之后**可能**就没法观看了。