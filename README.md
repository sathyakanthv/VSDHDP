# VSDHDP
VLSI Hardware Development program <br>
This repository contains the entire flow from the RTL design to GDSII. It covers all the steps including Synthesis (includes post-Synthesis analysis), Floorplanning, Placement, clock tree synthesis - CTS, and Routing. <br>
**Day 0**: Created the Github repository. <br>
**Day 1**: Installation of the tools required. <br>
First step: Installed VMBox and Ubuntu 20.04. <br>
Hardware Requirements set as 8GB RAM, 70 GB HDD for the virtual machine. <br>
__Yosys Installation Flow__
 <br>
```
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
```
Invoke using yosys <br>
![yosys](https://github.com/sathyakanthv/VSDHDP/assets/4946509/9b4ee86d-35b4-403d-851e-c34492411a89)
__iverilog Installation Flow__
```
sudo apt-get install iverilog
```
Invoke using iverilog
![iverilog](https://github.com/sathyakanthv/VSDHDP/assets/4946509/b98bd0de-8f4f-4e14-9214-31cd299fa0b2)
