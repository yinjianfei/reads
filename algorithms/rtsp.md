netty
https://github.com/pedroSG94/rtmp-rtsp-stream-client-java/blob/master/rtsp/src/main/java/com/pedro/rtsp/rtsp/RtspClient.java


https://www.jianshu.com/p/049d03705a81


ffmpeg -i video.webm -vf "select=eq(pict_type\,I)" -vsync vfr thumb%04d.jpg -hide_banner


ffmpeg -i "C:\Applications\FFMPEG\aa.mp4" "frames/out-%03d.jpg"


ffmpeg -i video.avi -vf "fps=25" frames/c01_%04d.jpeg


ffmpeg -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -vf "fps=25" frames/c01_%04d.jpeg


ffmpeg -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -f image2 -updatefirst 1 img.jpg


ffmpeg -rtsp_transport tcp -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -f image2 -vf fps=fps=1 hello/img%03d.png


ffmpeg image2pipe


ffmpeg  -r 1 -i 2.avi -r 1 -c:v pam -f image2pipe pipe:1 | magick - +write info: -evaluate-sequence mean x1_%06d.png


ffmpeg -rtsp_transport tcp -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -f image2 -vf fps=fps=1 hello/img%03d.png

ffmpeg -rtsp_transport tcp -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -c:v png -f image2pipe -

ffmpeg -rtsp_transport tcp -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -f image2 -

ffmpeg -rtsp_transport tcp -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -c:v png -f image2pipe pipe:1 > /tmp/fifo


ffmpeg -rtsp_transport tcp -i rtsp://admin:admin@10.10.32.175 -qscale:v 1 -y intermediate1.mpg < /dev/null &


fmpeg -rtsp_transport tcp -i rtsp://admin:admin@10.10.32.175 -f image2pipe -y aa < /dev/null &

ffmpeg -rtsp_transport tcp -i rtsp://admin:Hik12345+@192.168.19.113:554/h264/ch33/main/av_stream -f image2pipe -

  ffmpeg -xerror -nostdin -rtsp_transport tcp -i rtsp://Y02C011908150036:059dce8b83f95c5660f81343a8089050@10.10.200.28:8086/live -f image2pipe -y /tmp/fifo


ffmpeg -nostdin -rtsp_transport tcp -i rtsp://admin:admin@10.10.32.175 -f image2pipe -y /tmp/fifo

ffmpeg -rtsp_transport tcp -i rtsp://admin:admin_123@10.10.200.178 -f image2 -
Port 8095
BindAddress 0.0.0.0
MaxHTTPConnections 2000
MaxClients 1000
MaxBandwidth 500000
CustomLog -
RTSPPort 7654
RTSPBindAddress 0.0.0.0
<Stream video>
 Format rtp
 File "/path/video.mp4"
</Stream>


10.10.200.178
