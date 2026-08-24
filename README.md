# About
I have recently been researching the c220 camera from Eufy and have found multiple security vulnerabilities in the camera that have been properly disclosed. I will post more about these here once allowed by Eufy as one of them is extremely helpful when it comes to reverse engineering the firmware. I have decided to post things about the camera that are currently not under responsible disclosure through Eufy to help with future security researchers on the Eufy C220 or other similar Eufy cameras.
# PCB
The PCB on the C220 camera has a UART pin-out that is specifically compiled to not drop into a shell on boot. (Will post pictures of the PCB later). You are able to pull the firmware off of the SPI flash chip and the current version of the firmware as of 8/23/2026 is unencrypted and can be turned into a SquashFS filesystem using binwalk. The camera is using the Rockchip RV1103B.
# Firmware
Inside the firmware I was able to pull the hash for the root password which ends up being rockchip. While this cannot be used to just strictly login to the device off the shelf due to it not immediately dropping you into a shell it could be useful for people later on. I would also like to note for anyone doing security research that they tend to do their own home-made versions of encryption that can be reverse engineered. 
# Tech Stack
## Ports
The only port open on the device is UDP port 32108. This seemed to be the main way the camera is communicated with over the phone on the local network. The phone initially will send a LOCAL_LOOKUP packet to the broadcast address with the device identifier known in the camera as a p2p_did. The camera will then respond with its own IP and an ephemeral port to continue communication. From here after exchanging a few more packets you have a session with the camera in which you can send commands back and forth. 

## Wifi
The camera gets connected to the network one time on device registration. I have not finished analysis on this part but it seems like it spins up its own AP which your phone briefly connects to for the exchange of the WiFi password. The WiFi password is then stored in plaintext on the camera (not the biggest deal since there is no easy way to get onto the device even if you take it apart but kind of shows the security by obscurity of the camera).

## Bluetooth
It does not seem to use bluetooth at all for communication but I have not fully finished my bluetooth analysis. I will update if I find anything related to bluetooth. 
