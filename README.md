# HTW-SN-RIOT-LED
using the RIOT-OS to controll samr21-xpro devices with led's via Multicast 

This Repository is part of the Forschungsseminar at HTW Dresden. It Contains the RIOT-OS in an modified form. This Readme is seperated in 2 Parts. The First one are the steps and background how to reproduce this Repo/Project. Second one is how to use the Project to Controll the Led's via Multicast.  If you have an trouble or understanding issues feel free to email me at gabor-schulze@posteo.de 

in this Document Microcontrollers are sometimes called Node !



## Part I Use Riot's Borderrouter and Gcoap examples to create an Nodebased network and run commands with the coap-client.

### preparation

I used to Do this on an fresh Ubuntu 16.04 32bit and have worked with somebody who used Ubuntu 18.? So the Steps here should work with 16.04, in the 18 version there where some Differences, espacialy for the arm-toolchain (wich is used to compile code for the samr21). if you having trouble with ubuntu 18 you should see the work of wenching du (link einfügen), he used to work with the saame microcontroller and the ubuntu 18. I worked with an native Ubuntu and not with an virtual machine. you can reproduce the steps on an virtual machine and flash the microcontroller (remember to open the usb ports to the virtual machine), but as i experienced there was some trouble with networking and the virtual machine so if the network via usb connection to the Borderrouter is not working you should switch to an nativ Ubuntu.

If starting with an fresh Ubuntu version the first thing you wanna do is to update the preinstalled packeges and the packagelist itself

`sudo apt-get update`

`sudo apt-get upgrade`

To work with riot-os some addition packeges are needed, install them as well

`sudo apt-get install Git, python, python-pip, wget, unzip, automake, make, libtool, pkg-config, libhidapi-dev, gdb-arm-none-eabi, libudev-dev, python-serial, python3-serial, openocd`

Now with all dependecys installed, the next step is to Clone the Riot-OS. Be aware that Riot-Os is constantly on development and changes can appear. The Riot-Os Version here is from 2018 

git clone https://github.com/RIOT-OS/RIOT.git

Riot-Os Provides 2 important tools for the Connection with the running Microcontrollers. These have to be builded first. All you need to do is to navigate to the given Folders and run the "make clean" Command.

`cd .../RIOT/dist/tools/ethos`

`make clean all`

`cd .../RIOT/dist/tools/uhcpd`

`make clean all`

### the Border Router

Now all neccessery steps are done to Compile the Exapmles that Riot-Os provied such as the BorderRouter and the gcoap exapmles.
Background :
The samr21-xpro Controllers working with the IEEE 802.15.4 Protocol which is super energy saving, but is not compatible with wlan or anything else an "normal" computer or laptop provides. So the Connection to the Microcontroller have to be via USB because its the only usable Interface from our point of view. The Microcontrollers on the other hand can communicate over the IEEE 802.15.4 Protocol and thats where the BorderRouter takes place. He is connected to the Ubuntu via the USB and Communicates with the LED-Microcontrollers over the network. So he works as an Router and forwards all the Traffic between pc and Microcontrollers. For This purpose you give the Borderrouter an IP-Prefix wich the BR is using to create his own subnet, but later more .

So the Borderrouter is the connection to the running Microcontrollers. To make on of the Nodes an Microcontroller plug it on your pc and navigate to the /RIOT/examples/gnrc_border_router 

`cd .../RIOT/examples/gnrc_border_router/`

there is an makefile wich we can use to generate an borderrouter node. Riot is not only working with the samr21-xpro , so you should change the BOARD variable in the makefile to samr21-xpro, if not you can alternatively call 

`export BOARD=samr21-xpro`

to set an bash enviroment variable. but you have to do this every time you open an new terminal.

After telling Riot what kind of board is used just call "make all" in the gnrc_border_router folder

`make all`

This Line generates the binary code for the Samr21-xpro. this Step is only needed after downlaoding Riot or after changes to the Codebase or Makefile. Now the binary code has to be moved to the Node. This process is called flashing the Node and is done in via the "make flash" commandthe same folder

`make flash`

now you should see some flashing lights on the node and some percentage numbers on the terminal. the Code is now on the Node and starts running. if you remove the Node from your computer its turned off. After pluging it back again the Code Starts to run again from the beginning. Watch out to have only 1 microcontroller connected to the computer when flashing. Its Possible to connect to the Node via the terminal with the "make term" command. The make term Command in the BorderRouter folder starts another command "sudo sh /home/gabor/RIOT/dist/tools/ethos/start_network.sh /dev/ttyACM0 tap0 2003:5f:6e1d:c810::/60" which is actualy starting the interactive Console for the Borderrouter. So you can call this line instead and change parameters if needed. The "/dev/ttyACM0" describes which USB will be used. The first connected microcontroller is "ttyACM0" the second is "ttyACM1" and so on, if you have Multicple Nodes Connected to the Computer you can target for which microcontroller you want to start the interactive terminal. The "2003:5f:6e1d:c810::/60" Ip-Adress at the end tells the borderouter which subnet it should use for new connected microcontrollers.

`make term`

=> 

`sudo sh /home/gabor/RIOT/dist/tools/ethos/start_network.sh /dev/ttyACM0 tap0 2003:5f:6e1d:c810::/60`

That are all the steps you need to do tho generate an Borderrouter Node. The subnet will be important later when configuring all devices to reachabile over the internet

### the coap node

to generate an coap node move to the "gcoap" folder and repeate the steps neccessary for the BorderRouter

```
cd .../RIOT/examples/gcoap/
make all
make flash
make term
```

this is just the coap node provided by 

## Part II Steps neccessery to use RIOT-OS and the LED on Microcontroller Project https://github.com/HTWDD-RN/Sensornetzdemo to controll the samr21-xpro via Multicast.
