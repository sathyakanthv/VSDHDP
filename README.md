# VSDHDP
VLSI Hardware Development program <br>
This repository contains the entire flow from the RTL design to GDSII. It covers all the steps including Synthesis (includes post-Synthesis analysis), Floorplanning, Placement, clock tree synthesis - CTS, and Routing. <br>
**Day 0**: Created the Github repository. <br>
**Day 1**: Installation of the tools required. <br>
First step: Installed VMBox and Ubuntu 20.04. <br>
Hardware Requirements set as 8GB RAM, 70 GB HDD for the virtual machine.
Yosys installation <br>
$ git clone https://github.com/YosysHQ/yosys.git <br>
$ cd yosys <br>
$ sudo apt install make (If make is not installed please install it) <br>
$ sudo apt-get install build-essential clang bison flex \ <br>
    libreadline-dev gawk tcl-dev libffi-dev git \ <br>
    graphviz xdot pkg-config python3 libboost-system-dev \ <br>
    libboost-python-dev libboost-filesystem-dev zlib1g-dev <br>
$ make config-gcc <br>
$ make <br>
$ sudo make install <br>
Invoke using yosys <br>
![yosys](https://github.com/sathyakanthv/VSDHDP/assets/4946509/9b4ee86d-35b4-403d-851e-c34492411a89)
__Installation Flow__
```
$ mkdir yosys-master
$ cd yosys-master
$ git clone https://github.com/YosysHQ/yosys.git
$ sudo apt install make(installing make if you havent done it yet)
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ cd yosys-master/yosys/
$ make
$ sudo install make

if it doesn't work ( version mismatch might occur when combining other open software )
$ sudo apt install yosys
$ sudo apt upgrade
```
