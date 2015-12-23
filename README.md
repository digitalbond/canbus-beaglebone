# CANBus Hacking

## Required Materials
In order to play with CANBus using a Beaglebone Black you will need a beaglebone, a canbus transceiver cape, and a power supply. Power supplies are necessary because of additional requirements of the canbus cape and line.

For our class we purchased:
* http://www.logicsupply.com/cbb-serial/
* http://www.logicsupply.com/bb-black-c/
* http://www.logicsupply.com/pw-5v2a/

## Setup
Setup instructions for getting a Beaglebone Black ready for CANBus hacking fun

### Getting started
Follow the getting started instructions steps 1-3 on the beagleboard homepage at [beagleboard.org/getting-started](http://beagleboard.org/getting-started)

### (Optional) Boot from SDCard
We like to boot off of an SDCard rather than the onboard flash. Follow the [Update board with latest software](http://beagleboard.org/getting-started#update) instructions 
One-liner command for writing the image to an sdcard in linux (fill in your img path and your sdcard path):
```sh
sudo sh -c "xzcat /path/to/bone-debian-xxx.img.xz | dd of=/dev/sdX bs=1M"
```
The SDCard Image has a very small partition for the rootfs. You are probably going to want to increase this partition in order to have working space on the drive. Follow the instructions at http://www.howtogeek.com/114503/how-to-resize-your-ubuntu-partitions/ to increase the partition size to take advantage of all space available on your SDCard.

### Install dependencies
The remaining shell commands should be executed on your beaglebone. Connect via ssh and run:
```sh
apt-get install mercurial
```

### Download and build can-utils
When you have a recent Debian image for your Beaglebone Black (Debian Jessie 8.x) you can install the can-utils by just downloading the can-utils Debian package:
```sh
apt-get install can-utils
```

If not make sure your beaglebone has internet access and execute the following commands to build utilities directly on the beagle.
```
git clone https://github.com/linux-can/can-utils.git
cd can-utils
./autogen.sh
./configure
make
make install
```

You may get an error when running ./configure on line 11668. Edit the file with vim or nano and delete line 11668.

### Set up Nodejs socketcan
Nodejs comes pre-installed on the debian beaglebone image with the package manager, npm.

```
npm install -g socketcan
```

Test if the module installed correctly at a node prompt:

```js
> var can = require('socketcan');
> can
```
If the can object is displayed the dependency is working correctly.


### Set up Python-Can
```
cd ~/
hg clone https://bitbucket.org/hardbyte/python-can
cd python-can
python setup.py install
```

Create a file ~/.canrc that contains the following text
```
[default]
interface = socketcan_ctypes
```

Test that python-can is working correctly. Start up a python shell with `python`:
```python
>>> import can
>>> help(can)
```
If the help message is displayed python-can is ready to be used.
