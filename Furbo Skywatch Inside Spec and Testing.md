#### Skywatch Innovation Inc.
##### Company Confidential



# Furbo Camera Skywatch Inside Integration Requirements (POC) 
---

## 1. Hardware Requirements 

Flash Memory
---
Available flash memory at least 5MB after required kernel features / software packages are installed.

RAM
---
Available RAM at least 15MB after the camera is booted up with standard firmware.
```bash
Binary Path:
/furbo2/system/skwi_init
```


Video
---
Video Codec H.264 is required. Resolution at more than 720p is strongly preferred. 


Audio
---
Audio Codec AAC is preferred / PCMU, G.711, or GSM-AMR, adjustable bitrates is strongly preferred.

Audio out if available, PCM device with ALSA compatibility is preferred; PulseAudio is acceptable. 


WiFi 
---
If available, it should be compatible with popular brands of routers, and be stable enough for continuous streaming, and automatically recover from router WiFi failures.


SD Card
---
If supported, the file system should be robust enough to withstand power outages, network outages, and camera crashes, for example. The files should preferably be tagged in UTC/GMT time zone. 


## 2. Software Requirements 

### OS
Operating System ARM-based Linux preferred. 

### Software Package
Software packages OpenSSL (libcrypto, libssl), curl, latest stable version.

### VPN
Kernel features TAP / TUN interface should be turned on in the Linux kernel (for VPN connection).

### Streaming Protocol
RTSP (over HTTP preferred) is required, with FLV over HTTP support. MPEG-DASH or RTMP is also preferred. We need single channel transport to establish tunnel, If unavailable, we run a proxy server to hook up built-in RTSP server.

### Streaming Configuration
Support at least two (preferably three or more) independent streaming resolutions / Codec configurations that can simultaneously run at the maximum resolutions. 

### Streaming Resolution
Default resolution should be set at the highest. 
Support at least 5 simultaneous client connections. 
Support adjustable streaming video quality with variable bit rates. 


### Streaming Recording
Video codec: H.264
Audio codec: AAC (upsampling from 16kHz to 48kHz)

### Authentication
User authentication authenticate users with MAC address and UUID.

### Command Server
HTTP CGI interfaces, we provide skeleton to get skwi_httpd and skwi_cgi with needed libraries (depend on what command services are needed) linked.

~~### Event Sources
Cameras should be able to detect events based on video motion. 
also prefer to be able to create a video event through API or CGI calls to the cameras.~~ 

~~### Push Events
Push Events Video and image events can be pushed from the camera to a remote server. HTTP preferred, FTP acceptable. The file name prefix should be configurable, or the file name should contain timestamp and references to the camera ID or MAC address.~~

~~The video event should start no less than 5 seconds before the event, and the event duration should be no less than 10 seconds. 
The minimum interval between consecutive events should be configurable by the user.~~

### Others 
Robust NTP (Network Time Protocol) support is required; if not, we prefer to set the NTP server to portal.skywatch24.com. Need to have a method to scan for all cameras and acquire their MAC addresses in the LAN without authentication. DHCP should be turned on by default. 

### A. LED Control 
In order to indicate the status of Skywatch service connectivity, we recommend the following LED behavior, 
Steady green when the camera has aquired a valid IP address. Flash green at one second interval when the network interface TAP or TUN is up. 

This is usually achieved by modifying the default camera LED control to include this additional behavior, as there should be only one LED owner process on the camera. 
We require that TAP / TUN interfaces enabled in the Linux kernel. 

### POC Demo Deliveries
Skywatch fw porting pack
https://drive.google.com/open?id=12ssqcHlfkOQFRnLFrrPgydaEnkg7Ybyz

Skywatch demo SDK for both Android and iOS
https://drive.google.com/open?id=1REycKBFM7NycJj51I4352UlAtnY0BgAh

Skywatch demo SDK app integration guide
https://hackmd.io/@hgD7N6KFTuy7_dKeJjuhig/SJWWXMZor

Skywatch inside POC spec for Furbo
https://hackmd.io/ezkrY_Z2QmSA4hE387DZMQ

Skywatch Command Server
https://drive.google.com/open?id=1EhIyfJ_3mXrOzr0gqWvEcHf2AAW34DtO



#### Phase 1 CPU and Memory usage information for each module

skwi_init - %CPU: 0.0 / %MEM: 1.0 (pmap: 364 KB)
skwi_bootstrap - %CPU: 0.0 / %MEM: 1.4 (pmap: 1064 KB)
openvpn - %CPU: 3.5 / %MEM: 2.8 (pmap: 1684 KB)
audio_proxy - %CPU: 0.3 / %MEM: 1.6 (pmap: 896 KB)
p2p_client - %CPU: 0.0 / %MEM: 1.9 (pmap: 884 KB)
rtsp_proxy - %CPU: 5.1 / %MEM: 5.7 (pmap: 5068 KB)
Total: %CPU: 8.9 / %MEM: 14.4 (pmap: 9.72 MB




| Test# | Connect time(s)(Relay) | Connect time(s) (P2P) | Connection Type | FPS |
| ----- |:---------------------- |:--------------------- |:--------------- |:--- |
| 10    | 1.635                  | 6.24                  | P2P             | 15  |
| 9     | 1.424                  | 6.693                 | P2P             | 15  |
| 8     | 1.345                  | 5.202                 | P2P             | 15  |
| 7     | 0.965                  | 6.39                  | P2P             | 15  |
| 6     | 1.423                  | 5.866                 | P2P             | 15  |
| 5     | 1.427                  |                       | Relay           | 15  |
| 4     | 1.383                  | 6.718                 | P2P             | 15  |
| 3     | 0.896                  | 5.708                 | P2P             | 15  |
| 2     | 1.349                  | 6.466                 | P2P             | 15  |
| 1     | 1.127                  | 6.807                 | P2P             | 15  |








device id: FUB0C090D55656
uuid: 0597d1cb50617a-4655-4230-4330-ad5d29

device id: FUB0C090881CE2
uuid: 0597d1cba0d970-4655-4230-4330-d0ef87

device id: FUB0C09088B171
uuid: 0597d1cbf4f589-4655-4230-4330-4342bc 