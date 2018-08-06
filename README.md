# IoT Hub to Pi

This is a short, base level description of how to connect a Raspberry Pi to Azure IoT Edge. This is mostly an amalgamation of other peoples tutorials- however once I have a full process flushed out, I will add more of my own notes, and configure as much as I can through the Azure Portal.

## Prerequisites

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

Copy [this](https://github.com/khilscher/IoTHubPiHackathon/blob/master/SenseHat_IoTHub_Http_Lab_Key.py) python code into a new text file, and save it as `SenseHat_IoTHub_Http_Lab_Key.py`. Next, move this file from your desktop to the Raspberry Pi (for example, in the Documents). You can do this simply if you set up the ssh or virtual desktop, as you can copy-paste or drag and drop the file over to the location.

## Create an IoT Hub and Device

- Navigate to the Azure Portal, and in the search bar, search for Hub. Click on the service 'IoT Hub'. Then, click the + in the corner to create a new one. I suggest using East US, and the free or S1 subscriptions provided in the free service. 

- Click on your newly created portal, and in the scrollbar on the left, find the 'Shared Access Policies' section under the settings. Click on this, then click on the bar that says iothubowner, and a bar should appear on the right. Find the 'connection key- primary string' and copy and save it somewhere (such as a text file). 

- Next, in the left bar, scroll down and find the IoT Edge bar, then add a new device

## Connect the Pi to the IoT Hub 

On the Raspberry Pi and type in the command line:
```sh
cd Desktop
sudo nano SenseHat_IoTHub_Http_Lab_Key.py
```
- Scroll down and find the line that says "Connection string = " and paste in the Connection Key you previously saved. (Azure Portal -> IoT Hub -> Shared access policies -> iothubowner -> Connection string-primary key)
- Just under that, set "deviceID = " to the name you previously created

## Test sending messages

Try running the python script by typing the following:
```sh
python SenseHat_IoTHub_Http_Lab_Key.py
```

From the hub, click on the device name. From here, navigate to the 'Message to Device' tab. In the message body tab, type a message (Hello World!), then in the top left, click send message. 

## Clean Up

Delete the resource group
