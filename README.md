# Tinycar
The Tinycars are small 1:87 scaled car platforms used for research in [miniature autonomy](https://www.frontiersin.org/articles/10.3389/fnbot.2022.846355/full).
Using on-board cameras, several concepts for autonomous driving can be tested and developed. 

This repo makes the Tinycar fully open source to gather everything needed to understand, troubleshoot or building new cars. 

## General Idea
Like the [Tinycar CM4](https://autosys-lab.de/platforms/2021miniaturplatform/), this new version of the Tinycar follows the same vision. Bringing autonomous driving into the smallest scale possible to have a testing platform which lies between simulation and fully sized vehicles. However, compared to the Tinycar CM4, this version tries to go much smaller in size to overcome the issue of hitting traffic lights. Also creating a more robust steering rack for more precise steering actions. Since most of the space on the Tinycar CM4 is taken by the 18650 Cell and the Raspberry Pi CM4, the new version goes down and uses an ESP32 Microcontroller instead. This comes at the cost of much lower performance power, but frees up space and lowers the energy consumption. So the new version does not replace the Tinycar CM4 but is a new addition to the family. 

The Tinycar is designed to be used with an ESP32 S3, which allows to accelerate ML needed operations. So small neural networks for end-to-end self driving can be compiled to run on the ESP32 or for rapid prototyping, the camera image (from an OV2640) of the car can be streamed in real-time over RTP to a more powerful computer. An external antenna should improve the Wi-Fi connection for the image streaming. 

## Controlling the car
### Library
You can use [tinycar_lib](https://github.com/danielriege/tinycar_lib) to communicate with the car. It can be used in C++ and Python alike. Basically it only exposes user-friendly methods to create firmware compatible protocol messages (TCCP and TCFP). Additionaly it also re-assembles the fragmented camera image into an openCV mat and provides with some diagnostic telemtry data (like frame latency, interarrival jitter etc.). This library is also used in the tinycar_runtime. 

### Firmware
The car runs using the [tinycar firmware](https://github.com/danielriege/tinycar_firmware).

Commands are sent to the car by using a custom protocol on top of UDP:
##### TCCP (TinyCar Control Protocol)
Includes different message types like: 
- car control (speed, steering angle and led state)
- telemtry (battery voltage, wifi rssi, camera fps)
- stream control (packet loss for congestion avoidance which is not yet implemented)
- rtt (timestamp for frame lateny measurement)

##### TCFP (TinyCar Frame Protocol)
Heavily inspired by [RTP](https://datatracker.ietf.org/doc/html/rfc3550).
Includes two message types. A sender report similar but simpler than RTCP and an header, which includes metadata for a frame fragment. 

### Simulation
To simulate driving behavior, use [tinycarlo](https://github.com/danielriege/tinycarlo). It is a simulation environment allowing to test and develop control software for the tinycar. It is wrapped inside an [Farama Gymnasium](https://gymnasium.farama.org/index.html) to allow easy training of RL agents. Inside the the examples folder of the tinycarlo repo, several different control algorithms can be found. The simulation also allows for the use of real-world environments, like at [HAW Hamburg](https://autosys-lab.de/platforms/2020h0streetplatform/) which uses [overhead cameras](https://github.com/autosys-lab/overhead_tinycar_tracking) to track the tinycar to localize the car (necessary for a ground truth in the simulation). 

### Runtime
The [tinycar runtime](https://github.com/danielriege/tinycar_runtime) is a cross-platform GUI application written in C++ for optimal performance. It is always work in progress until it can control the cars as Level 5 autonomous driving (probably never reached). Until then it can demonstrate the capabilities of the car (like road marking detection) and provide basic functionality like remote control and camera data collection. 

## Example Application
This video can demonstrate that it is possible to bring neural networks for autonomous driving down to a 1:87 scale and still be run in real-time:

[![Alt: Video demonstrating a road markings detection on the Tinycar CM4](https://img.youtube.com/vi/025U4egWtLs/0.jpg)](https://www.youtube.com/watch?v=025U4egWtLs)


