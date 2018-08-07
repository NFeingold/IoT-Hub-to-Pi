# IoT Hub to Pi

This is a short, base level description of how to connect a Raspberry Pi to Azure IoT Edge. This is mostly an amalgamation of other peoples tutorials- however once I have a full process flushed out, I will add more of my own notes, and configure as much as I can through the Azure Portal.

**NOTE:** If you stumble upon this, I have not finished documenting and testing all this-- it is just what I *think.* I am by no means an expert

## Prerequisites

- [Azure Subscription](https://portal.azure.com) 
- [Raspberry Pi](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/)
  - Power Cable (5v micro USB- This will likely come with the Pi)
  - 16+ GB MicroSD Card (And a computer to flash [Raspbian](https://www.raspberrypi.org/downloads/raspbian/))
  - [SenseHat](https://www.raspberrypi.org/products/sense-hat/) (Optional)
  
The following you will only need temporarily, while setting up the Pi:

- USB Wired Keyboard
- Monitor
- Cables necessary to connect Pi to Monitor
- USB Wired Mouse (Maybe optional)


*Note:* You will need to be able to connect the Raspberry Pi to the internet. It has an ethernet port onboard, and some (such as the Raspberry Pi 3) have a wireless chip.


## Set up the Raspberry Pi

Follow [this tutorial](https://blog.jongallant.com/2017/11/raspberrypi-setup/) on setting up a raspberry pi. It is a somewhat lengthy process, however you only ever have to do it once. It teaches you how to install the operating system, prepare things for this example, set up SSH, and a remote desktop which are __extremely helpful__. It is succinct and the best tutorial I have found. It suggests numerous peripherals, however, many are unnecessary. You only need what I previously mentioned.

## Install Azure IoT Edge on the Pi

I will be following [this tutorial](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux-arm) on setting up the Pi with IoT Edge. Feel free to follow their tutorial for more detail. If you do, [skip to installing a deployment](https://github.com/NFeingold/IoT-Hub-to-Pi/blob/master/README.md#install-a-deployment)

- Install the container runtime: 
```sh
# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f
```
- Install the Secuirity Daemon 
```sh
# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```
- Configure the Daemon 
```sh
sudo nano /etc/iotedge/config.yaml
```

Update the value of device_connection_string with the connection string from your IoT Edge device.
(IoT Hub -> IoT Edge -> Device Details -> Connection String Primary Key)
**Note: This is NOT the IoT Hub Connection String, this is the Device String**

To exit, press Ctrl+X, Y, Enter

- Restart the Daemon
```sh
sudo systemctl restart iotedge
```
If succesfull, when you navigate to the IoT Hub Edge Devices and click on your device, it should say 'connected' near the bottom

## Install a deployment 

Kind of... I'm working on it <br/>
Currently, we will be doing this manually using [this tutorial](https://github.com/khilscher/IoTHubPiHackathon/tree/master/3) that I will be translating into this document. You do __not__ need to follow that link, it is just there to give credit where it is due.

## Configure the Raspberry Pi to send messages to the IoT Hub

- Copy [this](https://github.com/khilscher/IoTHubPiHackathon/blob/master/SenseHat_IoTHub_Http_Lab_Key.py) python code into a new text file, and save it as `SenseHat_IoTHub_Http_Lab_Key.py`. Next, move this file from your desktop to the Raspberry Pi (for example, in Documents). You can do this simply if you set up the ssh or virtual desktop, as you can copy-paste or drag and drop the file over to the location.

## Create an IoT Hub and Device

- Navigate to the Azure Portal, and in the search bar, search for Hub. Click on the service 'IoT Hub'. Then, click the + in the corner to create a new IoT Hub. I suggest using East US, and the free or S1 subscriptions provided in the free service. 

- Click on your newly created portal, and in the scrollbar on the left, find the 'Shared Access Policies' section under the settings. Click on the bar that says iothubowner, and a window should appear on the right. Find the 'connection key- primary string' and copy and store it somewhere (such as a text file). 

- In the left navigation bar, scroll down and go to IoT Edge, then Add IoT Edge Device. All you have to do is name it.

## Connect the Pi to the IoT Hub 

On the Raspberry Pi and type in the command line:
```sh
cd Desktop
sudo nano SenseHat_IoTHub_Http_Lab_Key.py
```
- Scroll down and find the line that says "Connection string = " and paste in the Connection Key you saved. <br/> (Azure Portal -> IoT Hub -> Shared access policies -> iothubowner -> Connection string-primary key)
**Note: This is NOT the device connection string, this is the IoT Hub connection string primary key**<br/>
- Just under that, set "deviceID = " to the name you created

From here, press Ctrl+X, Y, Enter (Exit, Yes (Save), Exit)

## Test sending messages

Try running the python script by typing the following:
```sh
python SenseHat_IoTHub_Http_Lab_Key.py
```
*Note: You must be in the correct directory to run a file! Use cd to navigate to the correct folder* <br/>

- From the hub, click on the device name. From here, navigate to the 'Message to Device' tab. In the message body tab, type a message (IE: Hello World!), then in the top left, click send message. 

If succesful, the Sense Hat should display the message. As well, the command line interface should also display the message. This refreshes every 5 seconds, so there might be a small delay.

## Clean Up the Pi

- To stop the python script, press Ctrl+C

I reccomend keeping all the local files, so you can reuse them in the future. They take up very little storage. However, if you want to delete them, all you have to do is navigate to the folder where you saved the python script and delete it. 

- To remove IoT Edge runtime:
```sh
sudo apt-get remove --purge iotedge
sudo docker ps -a
sudo docker rm -f edgeHub
sudo docker rm -f edgeAgent
sudo apt-get remove --purge moby
```

*Note: I have not yet tested removing anything-- this is just the documentation I found [here](https://docs.microsoft.com/en-us/azure/iot-edge/quickstart-linux#clean-up-resources)*

## Clean up Azure

- Delete the resource group that the Hub was built in. <br/> This can be done by navigating to the resource group folder, then clicking on the resource group, then clicking delete at the top. 
