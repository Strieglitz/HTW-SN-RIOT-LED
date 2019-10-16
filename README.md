# HTW-SN-RIOT-LED
using the RIOT-OS to controll samr21-xpro devices with led's via Multicast 

This Repository is part of the Forschungsseminar at HTW Dresden. It Contains the RIOT-OS in an modified form. This Readme is seperated in 2 Parts. First on is how to use the Project to Controll the Led's via Multicast. The Second one are the steps and background how to reproduce this Repo/Project. If you have an trouble or understanding issues feel free to email me at gabor-schulze@posteo.de



Part II Steps neccessery to use RIOT-OI and the LED on Microcontroller Project (link einfügen) to controll the samr21-xpro via Multicast

I used to Do this on an fresh Ubuntu 16.04 32bit and have worked with somebody who used Ubuntu 18.? So the Steps here should work with 16.04, in the 18 version there where some Differences, espacialy for the arm-toolchain (wich is used to compile code for the samr21). if you having trouble with ubuntu 18 you should see the work of wenching du (link einfügen), he used to work with the saame microcontroller and the ubuntu 18. I worked with an native Ubuntu and not with an virtual machine. you can reproduce the steps on an virtual machine and flash the microcontroller (remember to open the usb ports to the virtual machine), but as i experienced there was some trouble with networking and the virtual machine so if the network via usb connection to the Borderrouter is not working you should switch to an nativ Ubuntu.

If starting with an fresh Ubuntu version the first thing you wanna do is to update the preinstalled packeges and the packagelist itself
sudo apt-get update
sudo apt-get upgrade

To work with riot-os some addition packeges are needed, install them as well
sudo apt-get install Git, python, python-pip, wget, unzip, automake, make, libtool, pkg-config, libhidapi-dev, gdb-arm-none-eabi, libudev-dev, python-serial, python3-serial, openocd

Now with all dependecys installed, the next step is to Clone the Riot-OS. Be aware that Riot-Os is constantly on development and changes can appear. The Riot-Os Version here is from 2018 
git clone https://github.com/RIOT-OS/RIOT.git

Riot-Os Provides 2 important tool for the Connection with the running Microcontrollers. These have to be builded first. All you need to do is to navigate to the given Folders and run the "make clean" Command.
cd .../RIOT/dist/tools/ethos
make clean all
cd .../RIOT/dist/tools/uhcpd
make clean all

Now 
