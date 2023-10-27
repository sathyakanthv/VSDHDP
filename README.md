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
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make 
$ sudo make install
```
Invoke using yosys <br>
![yosys](https://github.com/sathyakanthv/VSDHDP/assets/4946509/9b4ee86d-35b4-403d-851e-c34492411a89)
__iverilog Installation Flow__
```
sudo apt-get install iverilog
```
Invoke using iverilog
![iverilog](https://github.com/sathyakanthv/VSDHDP/assets/4946509/b98bd0de-8f4f-4e14-9214-31cd299fa0b2)
__Gtkwave installation__
```
sudo apt update
sudo apt install gtkwave
```
![iverilog](https://github.com/sathyakanthv/VSDHDP/assets/4946509/044cefd0-b43d-44ef-909c-90b6eb519aaf)

**Assignment**: Introduction to Verilog RTL Design and Synthesis <br>
**Definitions**
**RTL Design**: RTL Design is checked for adherence to spec by simulating the design. The Design is a verilog code(or a set of verilog codes) which has the intended functionality to meet the requirements. <br>
**Test bench**: Test bench is the setup to apply stimulus (test_vectors) to the design to check its functionality. 
**Note**: Simulator checks for changes on input signal. No change in input means, no change in output. 

**Implementation example**:
1. The Design and Test bench are the inputs to the simulator which generates a VCD file (Value Change Dump). This is then processed by GTKWave to obtain the waveform which would enable us to verify the functionality.  <br>
2. The verilog design and the library files were cloned from : https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git <br>
3. The Design we are using to test is a 2X1 Multiplexer. File name is good_mux.v and test bench is tb_good_mux.v <br>

**Invoke using iverilog**<br>
Syntax:<br> 
iverilog design_name.v test_bench_design_name.v <br>
./a.out //generates vcd file <br> 
gtkwave test_bench_design_name.vcd
```
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```
![good_mux_test](https://github.com/sathyakanthv/VSDHDP/assets/4946509/11e521a7-8fb0-4ed8-98db-538a50aacb11)

