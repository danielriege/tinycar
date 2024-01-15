# Tinycar
The Tinycars are small 1:87 scaled car platforms used for research in [miniature autonomy](https://www.frontiersin.org/articles/10.3389/fnbot.2022.846355/full).
Using on-board cameras, several concepts for autonomous driving can be tested and developed. 

This repo makes the Tinycar fully open source to gather everything needed to understand, troubleshoot or building new cars. 

## General Idea
Like the [Tinycar CM4](https://autosys-lab.de/platforms/2021miniaturplatform/), this new version of the Tinycar follows the same vision. Bringing autonomous driving into the smallest scale possible to have a testing platform which lies between simulation and fully sized vehicles. However, compared to the Tinycar CM4, this version tries to go much smaller in size to overcome the issue of hitting traffic lights. Also creating a more robust steering rack for more precise steering actions. Since most of the space on the Tinycar CM4 is taken by the 18650 Cell and the Raspberry Pi CM4, the new version goes down and uses an ESP32 Microcontroller instead. This comes at the cost of much lower performance power, but frees up space and lowers the energy consumption. So the new version does not replace the Tinycar CM4 but is a new addition to the family. 

The Tinycar is designed to be used with an ESP32 S3, which allows to accelerate ML needed operations. So small neural networks for end-to-end self driving can be compiled to run on the ESP32 or for rapid prototyping, the camera image (from an OV2640) of the car can be streamed in real-time over RTP to a more powerful computer. An external antenna should improve the Wi-Fi connection for the image streaming. 

## Controlling the car
### Firmware
The car runs using the [tinycar firmware](https://github.com/danielriege/tinycar_firmware).

Commands are sent to the car by using a custom protocol on top of UDP. Using the same procotol, the car will also send diagnostics and telemetry data back. Using a mDNS mechanism, cars on the local network can be discovered. RTP and RTCP are used to stream the camera images from the ESP to another receiver. 

Protocol definition follows as soon as firmware has v2 release...

### Runtime
The [tinycar runtime](https://github.com/danielriege/tinycar_runtime) is a cross-platform GUI application written in C++ for optimal performance. It is always work in progress until it can control the cars as Level 5 autonomous driving (probably never reached). Until then it can demonstrate the capabilities of the car (like road marking detection) and provide basic functionality like remote control and camera data collection. 

## Example Application
This video can demonstrate that it is possible to bring neural networks for autonomous driving down to a 1:87 scale and still be run in real-time:

[![Alt: Video demonstrating a road markings detection on the Tinycar CM4](https://img.youtube.com/vi/025U4egWtLs/0.jpg)](https://www.youtube.com/watch?v=025U4egWtLs)


