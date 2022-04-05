

* Install Raspberry Pi

* Run sudo raspi-config
    *  5 Localisation Options
    *  L2 Timezone
    *  Europe > Stockholm

## Docker
* sudo apt-get update && sudo apt-get upgrade
* sudo reboot
* sudo curl -sSL https://get.docker.com | sh

sudo usermod -aG docker pi

## Install docker-compose
sudo apt-get install libffi-dev libssl-dev
sudo apt install python3-dev
sudo apt-get install -y python3 python3-pip

sudo pip3 install docker-compose

## Enable the Docker system service to start your containers on boot
sudo systemctl enable docker

# Install pre-configured node-red
* Unzip docker folder in /home

## Build docker container
* sudo docker-compose build

## Start node-red container
* sudo docker-compose up -d

## Configure Node-RED

* Open http://IP-to-raspberry-pi/
* Double-click CHANGE ME node
* Update the configuration
Click on the blue rectangle just left of "Runs automatically at startup" to activate the new settings

Click Car charge tab.
Double click "get car status" node
Click on the edit button next to "Add new bluelinky..."
Fill in username, password, Pin and Vin for your Bluelink account
Double click the Car charge tab
Enable the tab by hitting "Disabled" button in the bottom.
Click Done
Click Deploy in top right corner



# Known problems
## Base charging is not fully optimized
