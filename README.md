# IoT Hub to Pi

This is a short, base level description of how to connect a Raspberry Pi to Azure IoT Edge. This is mostly an amalgamation of other peoples tutorials- however once I have a full process flushed out, I will add more of my own notes, and configure as much as I can through the Azure Portal.

## Prerequisites

Normally, I don't like having prerequisites in my tutorials, but there is hardware you will need, which comes with very well flushed out tutorials that I would do nothing differently. So, you will need:

- [Azure Subscription](https://portal.azure.com) 
- [Raspberry Pi](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
- Power Cable (5v micro USB- This will likely come with the Pi)
- 16+ GB MicroSD Card (And a computer to flash [Raspbian](https://www.raspberrypi.org/downloads/raspbian/))
- [SenseHat](https://www.raspberrypi.org/products/sense-hat/) (Optional)
  - You can alternatively use the [Emulated Sense Hat](https://www.raspberrypi.org/blog/sense-hat-emulator/) which comes pre-installed

*Note:* You will need to be able to connect the Raspberry Pi to the internet. It has an ethernet port onboard, and some (such as the Raspberry Pi 3) have wifi/bluetooth chips


## Set up the Raspberry Pi

Follow [this tutorial](https://blog.jongallant.com/2017/11/raspberrypi-setup/) on setting up a raspberry pi. It is a somewhat lengthy process, however you only ever have to do it once. It teaches you how to install the operating system, set up SSH, and a remote desktop which are __extremely helpful__. It is succinct and the best tutorial I have found. It suggests numerous peripherals, however, many are unnescesary. In addition to what I had previously mentioned, you must have a USB mouse, USB keyboard, a monitor, and any nescesary cables/converter to connect the Pi (which has a female HDMI port) to a monitor. Once you have the remote desktop set up, you will no longer need any of these. 

## Install Azure IoT Edge on the Pi

Follow [this tutorial](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux-arm) on setting up the Pi with IoT Edge

# Install a deployment 

Kind of... I'm working on it <br/>
Currently, we will be doing this manually using [This Tutorial](https://github.com/khilscher/IoTHubPiHackathon/tree/master/3) that I will be translating into this document

## Configure the Raspberry Pi to send messages to the IoT Hub

Copy [this](https://github.com/khilscher/IoTHubPiHackathon/blob/master/SenseHat_IoTHub_Http_Lab_Key.py) python code into a new text file, and save it as `SenseHat_IoTHub_Http_Lab_Key.py`.
