# Designing Twitch to Understand Low Latency Applications

> Twitch is an interactive live streaming service for content spanning gaming, entertainment, sports, music, and more.

## Requirements of Low Latency Video Streaming

Unlike video playback streaming requires the content of videos to be delivered in or close to real time. To measure latency in this scenario we need to measure from the time when the video is recorded by the camera to the time when it is rendered and displayed on the viewer's screen. This concept is referred to as **glass-to-glass** latency and can be seen as far back as the paper [A system for high precision glass-to-glass delay measurements in video communication](https://www.researchgate.net/publication/307516190_A_system_for_high_precision_glass-to-glass_delay_measurements_in_video_communication). 

The stored VODs within Twitch may utilize an object storage system like Amazon S3 and a CDN like CloudFront. The video is received through an RTMP (Real-Time Messaging Protocol) and then transcoded. Transcoding is the process in which the video is decoded from one format into various other formats like various resolutions and bitrates to help better support various network connection strengths and connected devices.  

_Note: Along wit Transcoding, there are also the processes of Transmuxing where deliver format changes without any changes to encoding, and Transrating where the bitrate is changed._

## Why Video Streams Lag

- Distance
  -  The physical distance between the video's source and its recipient can contribute significantly to latency. The further the data has to travel, the longer it typically takes, especially if it passes through multiple routing points or hops. Leveraging Content Delivery Networks (CDNs) can mitigate some of this by caching content closer to end-users.

- Connection Quality
  -  The stability and bandwidth of both the broadcaster's and viewer's internet connections directly impact video streaming latency. A high-quality, stable connection can transmit data faster and more reliably, whereas a spotty or low-bandwidth connection may introduce delays or require the video to be streamed at a lower quality to prevent buffering.

- Server Load
   - When a platform experiences high traffic, the servers can become overloaded, leading to increased latency. Overburdened servers might struggle to process and deliver video streams promptly, causing delays or interruptions. Optimizing server infrastructure and using load balancers can help distribute the traffic and reduce the strain on individual servers.

- Streaming Protocol
  -  Different protocols handle streaming differently. For instance, RTMP (Real-Time Messaging Protocol) has historically been used for low-latency streaming, but it's less supported in modern browsers and applications. On the other hand, protocols like HLS (HTTP Live Streaming) or DASH (Dynamic Adaptive Streaming over HTTP) may introduce more latency due to their segment-based nature but are more widely supported and can adapt better to varying network conditions.

- Jitter Buffer
  -  A jitter buffer compensates for varying packet arrival times to ensure smooth playback. If packets of video data arrive at irregular intervals, the buffer will store them temporarily to create a steady stream. This, however, adds to the latency. The larger the buffer, the more delay it introduces, but it also results in smoother playback in environments where packet delay variation is high.

- Encoding
   -  Video encoding translates raw video data into a format suitable for streaming over the internet. The complexity of the codec, the quality settings, and the efficiency of the encoder hardware/software play a role in this latency. For instance, hardware-accelerated encoding might be faster than software-based encoding. Similarly, encoding at higher resolutions and bit rates may also take more time, adding to the delay.
