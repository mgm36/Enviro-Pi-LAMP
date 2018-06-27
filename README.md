# Enviro-Pi
BLE-based environmental monitoring system

## Supplies Needed 

1. Computer
2. 

## Arduino Device(s) Setup

Setup Arduino IDE per the Adafruit instructions at https://learn.adafruit.com/adafruit-feather-32u4-bluefruit-le/setup

Follow the assembly steps for the Adafruit Si7021 sensor, detailed at https://learn.adafruit.com/adafruit-si7021-temperature-plus-humidity-sensor

Connect the Feather to the Si7021:

| Feather        | Si7021          | 
| ------------- |:-------------:| 
| 3V     | 3V |
| GND     | GND       |
| SDA | SDA      |
| SCL | SCL      |	
| - | VIN      |

Within the Arduino IDE, install the libraries: SparkFun_Si7021_Breakout_Library.h (instructions at https://github.com/sparkfun/SparkFun_Si701_Breakout_Arduino_Library) and LowPower.h ((https://github.com/rocketscream/Low-Power)

Open the Arduino file at https://github.com/prattpi/Enviro-Pi/tree/master/Arduino/environ_monitor_si7021_lp connect your device to the computer's USB cable, and select the appropriate board and port from the IDE Tools dropdown. Compile and then upload the code to the Arduino. The serial monitor will output the device's setup actions for debugging if needed.

Repeat the setup instructions for any additional Arduino sensors desired. You will also need the hw addresses of each sensor to set up the Pi configuration in the next step. A quick way to gather the addresses is to let the Pi scan for them using the *hcitool* command, e.g.:

	pi@raspberrypi_ble:~ $ sudo hcitool lescan
	LE Scan ...
	F8:04:2E:87:52:2F (unknown)
	46:0E:A3:3D:5A:6A (unknown)
	46:0E:A3:3D:5A:6A (unknown)
	C8:69:CD:50:7A:0F (unknown)
	E4:6B:FA:9C:C0:A6 Bluefruit ES
	E6:BF:8E:87:53:19 (unknown)

Here you can see one of the sensor devices at the address *E4:6B:FA:9C:C0:A6*. Note these addresses for the next steps. 

## Raspberry Pi Installation

Download and install to SD card the latest full version of Raspbian from https://www.raspberrypi.org/downloads/raspbian/

Create an empty file named SSH and add your wireless credentials to a wpa_supplicant.conf file containing the following:

	ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
	update_config=1

	network={
	ssid="*your ssid*"
	psk="*your password*"
	key_mgmt=WPA-PSK
	}

Copy both files into the /boot directory of the SD card.

Put SD card in Pi and plug in power to boot up. The Pi should automatically connect to your wireless network. 

SSH to device with the Pi user, e.g. *ssh pi@192.168.50.199*, and enter the default password *raspberry*.

Run an update and upgrade: 

	sudo apt-get update && sudo apt-get -y upgrade  

Run sudo raspi-config and 1) choose "Change User Password", following the prompts to add your new password and then 2) set the Pi to boot to command line (choose "#3 Boot Options" -> B1 Desktop/CLI -> B1 Console) and 3) Choose localization options to select your timezone, and then save and reboot as prompted.

Now download and unzip the files into a new enviropi directory:

	cd ~pi && wget https://github.com/prattpi/Enviro-Pi/archive/master.zip && unzip master.zip -d /home/pi/enviropi && cd enviropi

Then run the installation file (you will need your sensors' hw addresses or you can manually edit the ini file later):

	./install.sh 
  
## Additional Notes

### Optionally may add external antenaa and/or screen 
### Instructions how to get the BLE device's hw addresses and handles
### How to configue different Arduino types in the ini file 
 
