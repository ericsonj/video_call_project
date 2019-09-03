# Video Call Project

## Stream video with Gstreamer

### VP8

Streaming video in VP8
```
gst-launch-1.0 -v videotestsrc ! vp8enc ! rtpvp8pay ! udpsink host=127.0.0.1 port=55000
```

Recive video in VP8
```
gst-launch-1.0 udpsrc port=55000 caps = "application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)VP8, payload=(int)96, a-framerate=(string)30" ! rtpvp8depay ! vp8dec ! videoconvert ! autovideosink

```

### H264

Streaming video in H.264
```
gst-launch-1.0 -v videotestsrc ! video/x-raw,width=800,height=600,codec=h264,type=video ! videoscale ! videoconvert \
! x264enc tune=zerolatency ! rtph264pay ! udpsink host=127.0.0.1 port=5600
```

Recive video in H.264
```
gst-launch-1.0 -vc udpsrc port=5600 close-socket=false multicast-iface=false auto-multicast=true \
! application/x-rtp, payload=96 ! rtpjitterbuffer ! rtph264depay ! avdec_h264 \
! fpsdisplaysink  sync=false async=false --verbose
```

Reference: https://patrickelectric.work/streaming_with_gstreamer/
s

