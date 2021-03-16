# docker 安装 
拉取镜像并运行
```

docker pull panjjo/zlmediakit

docker run -id -p 1935:1935 -p 8080:80 -p 8554:554 -p 10000:10000 -p 10000:10000/udp panjjo/zlmediakit

```

80 远程调用默认端口，通过URL进行，结果返回json格式的结果 
1000 通过ffmpeg推流rtp端口

# zlmediakit

使用C++编写的流媒体框架,主要使用视频的实时播放 
当前它只支持视频编码H264,音频编码acc

## 代理摄像头
/index/api/addStreamProxy
它的作用在于添加代理，不过它代理的是视频流，主要可以使用rtsp,rtmp,hls
示例   http://127.0.0.1/index/api/addStreamProxy?vhost=__defaultVhost__&app=proxy&stream=0&url=rtmp://live.hkstv.hk.lxdns.com/live/hks2

参数 	是否必选 	释意
secret 	Y 	api操作密钥(配置文件配置)，如果操作ip是127.0.0.1，则不需要此参数
vhost 	Y 	添加的流的虚拟主机，例如__defaultVhost__
app 	Y 	添加的流的应用名，例如live
stream 	Y 	添加的流的id名，例如test
url 	Y 	拉流地址，例如rtmp://live.hkstv.hk.lxdns.com/live/hks2
enable_hls 	N 	是否转hls
enable_mp4 	N 	是否mp4录制
rtp_type 	N 	rtsp拉流时，拉流方式，0：tcp，1：udp，2：组播

/index/api/delStreamProxy
作用：关于视频流代理
示例： http://127.0.0.1/index/api/delStreamProxy?key=__defaultVhost__/proxy/0

参数 	是否必选 	释意
secret 	Y 	api操作密钥(配置文件配置)，如果操作ip是127.0.0.1，则不需要此参数
key 	Y 	addStreamProxy接口返回的key

## 配置文件 
## 测试服务器
输入以下命令，查看是否有输出
```
ffmpeg -re -i "/path/to/test.mp4" -vcodec h264 -acodec aac -f rtp_mpegts rtp://127.0.0.1:10000
```
