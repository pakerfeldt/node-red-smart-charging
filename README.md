


# Node-RED Smart Charging

[Just looking for the step-b-step guide to get you started?](#step-by-step-guide)
# What is this (the one-liner)?
Software meant to run on Raspberry Pi to control your Easee chargebox for smart charging when electricity price is low.
# What is this (the longer version)?
This is a pre-configured Node-RED instance for managing smart charging of your car using the Easee charge box, along with instructions on how to get going. The instructions are essentially a step-by-step guide which should be sufficient even for non-tech-savvy persons.

Node-RED is a flow based programming tool popular in home automation. While this is meant to be a pre-configured setup working out-of-the-box, it can be extended to support new functionality quite easily.

# How does it work?
This software supports three ways of activating charging of your car.
## Smart charging
Activated when electricity price is relatively low. This is done by looking at the 30 day moving average of price and allow charging when current price is lower than average by X for some value of X that you can define yourself.

## Base charging
Base charging activates charging when your car battery is below X % (a level you define yourself) and when price is at its lowest but no later than 08.00 the following morning. Obviously for this to work, the software must be able to read that level from your car. Currently this software only supports Hyundai cars using Bluelink.

## Force charging
Through its user interface, you can also choose to activate charging manually regardless of current price. This is convenient if you require recharge as soon as possible or if you lend your charger to a friend.

## User interface
The software also comes with a user friendly interface where you can see electricity price for the most recent 48 hours, see state of your Easee charger, how this software plans to schedule next charging session and the ability to manually activate charging.

## Configuration interface
The software also comes with the Node-RED configuration interface. This is where you do some of the initial setup configuration and also where you can choose to change any or all of the logic that goes on behind the scenes.

# Requirements
You require at least:
* Easee chargebox
* Raspberry Pi + microSD card (4GB+)
* USB-C for powering the Raspberry Pi
* microSD card reader
* microHDMI -> HDMI (or other ways to access your Raspberry)

You might also want:
* Hyundai EV with Bluelink (to support Base charging), this was specifically tested with Hyundai Ioniq 5

# Step-by-step guide

## Prepare your Raspberry Pi
Install Raspbery Pi OS on your microSD card. This is easily done with Raspberry Pi Imager. In `Choose Image`, choose Raspberry Pi OS (32-bit), in `Choose storage` you select the microSD card which you have connected to your computer using a card reader. Hit `Write` to start the process. Raspberry Pi website has [further instructions](https://www.raspberrypi.com/software/).

## Access the Raspberry Pi
### Direct access
To gain access to your Raspberry Pi you can use a microHDMI -> HDMI cable and connect it to a monitor. Attach a keyboard/mouse and connect it to your local network.

### Remote access
Create a file called `ssh` in the root folder of the microSD card to enable ssh server. Then attach the Raspberry Pi to your local network. After powering up the Raspberry, figure out its IP and connect using SSH.
## Install software
The rest of the guide will assume you have direct access to your Raspberry Pi but works similarly with remote access.

Open up a terminal window.
### Set localisation
Run the following commands to set localisation.

`sudo raspi-config`

Choose 5, `Localisation Options`

Choose L2, `Timezone`

Pick your timezone, e.g. `Europe > Stockholm`
## Docker
Run the following commands to install docker and docker-compose.

`sudo apt-get update && sudo apt-get upgrade`

Now we need to reboot the Raspberry Pi.

`sudo reboot`

After booting up continue with:
`sudo curl -sSL https://get.docker.com | sh`

`sudo apt-get install libffi-dev libssl-dev`

`sudo apt install python3-dev`

`sudo apt-get install -y python3 python3-pip`

`sudo pip3 install docker-compose`

Enable the Docker system service to start on boot.

`sudo systemctl enable docker`

# Install pre-configured Node-RED
Replace 0.1 with the latest version or the version you want to install:

`VERSION=0.1 bash -c 'wget -c https://github.com/pakerfeldt/node-red-smart-charging/archive/refs/tags/v${VERSION}.tar.gz -O - | tar -xz && mv node-red-smart-charging-${VERSION} node-red-smart-charging'`

[See this page for list of releases](https://github.com/pakerfeldt/node-red-smart-charging/releases)

## Build docker container
`cd && cd node-red-smart-charging`

`sudo docker-compose build`

## Start node-red container
`sudo docker-compose up -d`

## Configure Smart Charging
Browse to the Node-RED admin interface on `http://[IP-to-raspberry-pi/]`. If you're working directly on the Raspberry Pi, it would be http://localhost/.

Double click the `CHANGE ME` node on the Configuration tab and update the variables according to your needs.

If you want to enable Base charging with Bluelink, go to `Car charge` tab and double click `get car status` node. Click on the edit button next to `Add new bluelinky...`. Fill in username, password, Pin and Vin for your Bluelink account. Click the red `Add` button and then `Done`.

## Start Smart Charging
Click `Deploy` in top right corner.

Go to `Configuration` tab and click on the blue rectangle just left of `Runs automatically at startup` to activate your settings.

Go to `Manage spot price` tab and click the blue rectangle just left of `Reload last 30 days` to fill Smart Charging with spot prices from last 30 days.

## All set!
Finally, Node-RED Smart Charging is running! To open the user interface, navigate to `http://[IP-to-raspberry-pi/]/ui` or, if working directly on the Raspberry Pi http://localhost/ui.

# Known problems
## Base charging is not fully optimized

