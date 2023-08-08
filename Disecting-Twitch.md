# Disecting Twitch to Understand Low Latency Applications

> Twitch is an interactive live streaming service for content spanning gaming, entertainment, sports, music, and more.

## Requirements of Low Latency Video Streaming

Unlike video playback streaming requires the content of videos to be delivered in or close to real time. To measure latency in this scenario we need to measure from the time when the video is recorded by the camera to the time when it is rendered and displayed on the viewer's screen. This concept is referred to as **glass-to-glass** latency and can be seen as far back as the paper [A system for high precision glass-to-glass delay measurements in video communication](https://www.researchgate.net/publication/307516190_A_system_for_high_precision_glass-to-glass_delay_measurements_in_video_communication). 

The stored VODs within Twitch may utilize an object storage system like Amazon S3 and a CDN like CloudFront. The video is received through an RTMP (Real-Time Messaging Protocol) and then transcoded. Transcoding is the process in which the video is decoded from one format into various other formats like various resolutions and bitrates to help better support various network connection strengths and connected devices.  

_Note: Along with Transcoding, there are also the processes of Transmuxing where deliver format changes without any changes to encoding, and Transrating where the bitrate is changed._

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

## How to Stream With Low Latency

As mentioned earlier RTMP and the related RTSP (S = Streaming, else same), are two examples of non-http protocols. RTSP works on the application layer and comes equipped with pausing and capabilities to support multiple data streams. It has however fallen out of favour since it is incompatible with HTTP and unusable by web-based streaming applications. RTMP was the gold standard once upon a time until recently when its scaling issues were exposed and it began to be replaced by more modern solutions. 

These more modern solutions come in the form of HTTP-based protocols. HLS, HTTP Live Streaming, use segmentation and CDN / web services to provide low latency live streaming applications that remain feature rich while still scaling better than RTMP. DASH, Dynamic Adaptive Streaming over HTTP is another such protocol that works in a very similar way. These protocols do come with minimum latency, however, equal to the amount of time a segment of video represents since for a segment to be played the entire segment must first be received. 

The new standard is WebRTC. This peer-to-peer protocol allows for millisecond latency and has incredible browser support.   
## Why Twitch uses RTMP and HLS

As I've talked about elsewhere there are Latency Sensitive Applications and Latency Critical Applications. Twitch finds itself falling into the first grouping. 

Twitch utilizes RTMP for video ingest, which allows support for ABS (Adaptive Bitrate Streaming) allowing streamers to adjust the quality of their streams based on their network conditions. Whilst also utilizing HLS for to-viewer streaming.HLS is more scalable, works well with CDNs, is supported by virtually all modern devices, and allows for adaptive bitrate streaming on the viewer's side. 

But why not WebRTC? WebRTC is designed for real-time communications and is primarily used for video calls, video conferences, and some live streaming scenarios. It offers ultra-low latency but has its challenges in scalability for large audiences, which makes it less ideal for platforms like Twitch that cater to massive concurrent viewerships. However, platforms that prioritize ultra-low latency, like some esports broadcasts or interactive streams, might look into WebRTC or similar solutions.

In summary, Twitch uses RTMP for stream ingest, and other protocols like HLS for delivering those streams to a wide audience. This hybrid approach leverages the strengths of each protocol instead of bearing the negatives of relying on a single solution to the problem. 

