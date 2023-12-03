# VSDHDP
VLSI Hardware Development program <br>
This repository contains the entire flow from the RTL design to GDSII. It covers all the steps including Synthesis (includes post-Synthesis analysis), Floorplanning, Placement, clock tree synthesis - CTS, and Routing. <br>
## Day 0
Created the Github repository. <br>
## Day 1
Installation of the tools required. <br>
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
**Test bench**: Test bench is the setup to apply stimulus (test_vectors) to the design to check its functionality. <br>
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
**Yosys Synthesis**: <br>
```
Syntax:
Invoke using 'yosys'
read_liberty -lib ../path_of_library_file/Library.lib
read_verilog design_name.v
synth -top module_name 
abc -liberty ../path_of_library_file/Library.lib
show
```
![yosys_21mux](https://github.com/sathyakanthv/VSDHDP/assets/4946509/bca6c9eb-251d-4752-8651-347b85b279b8)
**Synthesis to actual design**:
![yosys_show_goodmux](https://github.com/sathyakanthv/VSDHDP/assets/4946509/3806eb1b-90ba-4d65-83b3-4bb7dfc41050)
**Netlist Generation**:<br>
```
Syntax:
write_verilog design_name_netlist.v
write_verilog -noattr design_name_netlist.v
```
![yosys_netlist](https://github.com/sathyakanthv/VSDHDP/assets/4946509/c4cce608-365f-4371-9d76-3512ed37ca12)
**Simplified Netlist without attr**:
![yosys_mux_simplified_netlist](https://github.com/sathyakanthv/VSDHDP/assets/4946509/18422509-34ab-47f3-b244-28c65e238d6a)

## Day 2
Introduction to library file - notation and naming. <br>
1. The . lib (library) file consists of all the information on the electrical behaviour of the std cells used in the Chip design. It includes area, power for various standard cells. <br>
2. Different flavours are used as per the requirement of operation as in slow, medium or fast. <br>
3. Combinational logic delay in logic path determines the speed of operation of digital logic circuits. <br>
4. We have to consider setup time, and hold time and what happens if there are any violations.<br>
5. Difference between faster and slower cells and where to use which one. This selection of specific cells means that there are synthesis constraints. <br>
6. What is PVT (Process-Voltage-Temperature) and what variations or how the libs will be characterised to model the PVTs.<br>
7. What information is seen in a .lib file and how is it written and how to understand this. <br>
8. Hierarchical model and the flat model, differences in synthesis.<br>
9. Flipflops and how to utilise. <br>
```
Syntax:
Invoke using 'yosys'
read_liberty -lib ../path_of_library_file/Library.lib
read_verilog design_name.v
synth -top module_name 
abc -liberty ../path_of_library_file/Library.lib
show
```
**Synthesis**:
![submodule_show_1](https://github.com/sathyakanthv/VSDHDP/assets/4946509/c0fab1da-19dc-4ee8-8e70-5442b0653b0b)
**Netlist**:
![netlist_multiple_modules1](https://github.com/sathyakanthv/VSDHDP/assets/4946509/430c8479-a549-4bb3-844f-9b7160f6f05c)
**After using Flatten**:
```
Syntax:
flatten
write_verilog -noattr verilog_file.v
```
![flatten_netlist_show1](https://github.com/sathyakanthv/VSDHDP/assets/4946509/9916140a-1ac0-4dd3-bb3a-a21adbd60054)
**Netlist**:
![flatten_multiple_modules_netlist](https://github.com/sathyakanthv/VSDHDP/assets/4946509/1cb97b5a-cbce-416f-ac25-3e9cba741a86)
**For Submodule (individual)**:
```
Syntax(invoke yosys if you haven't already): 
read_liberty -lib ../path_of_library_file/Library.lib
read_verilog ../path_verilog_file/
synth -top submodule_name 
abc -liberty ../path_of_library_file/Library.lib
show
```
**Submodule Synthesis**:
![submodule_1_netlist](https://github.com/sathyakanthv/VSDHDP/assets/4946509/9ab47b5c-fd69-402a-8004-55d370e27f72)
**For Simulation (Asynchronous Reset)**:
```
Syntax: 
iverilog verilog_file_name.v tb_file_name.v
./a.out
gtkwave file_name.vcd
```
**Waveform for Asynchronous Reset**:
![tb_dff_asyncres_wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/0ea12b7c-a1a2-457b-a989-8448aaba5ed7)

**Synthesis for Asynchronous reset**:
1.Checking for synthesis, here we would specify the library for the dff using dfflibmap command. In this case, everything is in the same library, there is not much change noticed in the paths. <br>
```
Syntax: Invoke using yosys. Following are the commands:
read_liberty -lib ../path_of_library_file/Library.lib
read_verilog design_verilog_file.v
synth -top module_name
dfflibmap -liberty <path library>
abc -liberty ../path_of_library_file/Library.lib
show 
```
**Synthesis for Asynchronous reset-Show**:
![asyncres_show_synth](https://github.com/sathyakanthv/VSDHDP/assets/4946509/c280f072-8c79-42fd-aa32-c6f82c33d0c7)

**For Simulation (Asynchronous Set)**:
```
Syntax: 
iverilog verilog_file_name.v tb_file_name.v
./a.out
gtkwave file_name.vcd
```
**Waveform for Asynchronous Set**:
![tb_dff_async_set_wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/db8f9fa4-fdef-46a2-a81a-c986c61ac873)

**Synthesis for Asynchronous set**:
1.Checking for synthesis, here we would specify the library for the dff using dfflibmap command. In this case, everything is in the same library, there is not much change noticed in the paths. <br>
```
Syntax: Invoke using yosys. Following are the commands:
read_liberty -lib ../path_of_library_file/Library.lib
read_verilog design_verilog_file.v
synth -top module_name
dfflibmap -liberty <path library>
abc -liberty ../path_of_library_file/Library.lib
show 
```
**Synthesis for Asynchronous set-Show**:
![aync_set_show_synth](https://github.com/sathyakanthv/VSDHDP/assets/4946509/49835ac4-8a64-4ef3-90a8-ad382699b970)

**For Simulation (Synchronous Reset)**:
```
Syntax: 
iverilog verilog_file_name.v tb_file_name.v
./a.out
gtkwave file_name.vcd
```
**Waveform for Synchronous Reset**:
![tb_dff_syncres_wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/1b3a51d3-421e-4594-82bf-951c9c7fe69a)

**Synthesis for synchronous set**:
1.Checking for synthesis, here we would specify the library for the dff using dfflibmap command. In this case, everything is in the same library, there is not much change noticed in the paths. <br>
```
Syntax: Invoke using yosys. Following are the commands:
read_liberty -lib ../path_of_library_file/Library.lib
read_verilog design_verilog_file.v
synth -top module_name
dfflibmap -liberty <path library>
abc -liberty ../path_of_library_file/Library.lib
show 
```
**Synthesis for synchronous set-Show**:
![sync_reset_v](https://github.com/sathyakanthv/VSDHDP/assets/4946509/93320188-96d1-4e32-a02b-618509844129)

## Day 3: Combinational and Sequential optimisations (logical)
1. Area and power savings are achieved when we perform combinational and Sequential optimisations.<br>
2. The combinational optimisations are Constant propagation and Boolean optmisation. <br>
3. The Sequential optimisations are categorised as Basic (Sequential constant propagation) and Advanced (State optimisation, Retiming, Sequential logic cloning). <br>
**For Opt_check (Combinational)**:
```
Syntax: 
read_liberty -lib <library_file_path>
read_verilog <verilog_file>
synth -top <module_name>
opt_clean -purge
abc -liberty <library_file>
show
```
**Synthesis for Opt_check-Show**:
![opt_check_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/1645056e-7d39-48d8-b3b9-41417ee28bdb)

**For Opt_check2 (Combinational)**:
```
Syntax: 
read_liberty -lib <library_file_path>
read_verilog <verilog_file>
synth -top <module_name>
opt_clean -purge
abc -liberty <library_file>
show
```
**Synthesis for Opt_check2-Show**:
![opt_check2_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/c2d94f02-5776-4e6c-a715-c8ed287ad442)

**For Opt_check3 (Combinational)**:
```
Syntax: 
read_liberty -lib <library_file_path>
read_verilog <verilog_file>
synth -top <module_name>
opt_clean -purge
abc -liberty <library_file>
show
```
**Synthesis for Opt_check3-Show**:
![opt_check3_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/5bdc8e65-e0ce-4efc-ba9b-e81d1a7426be)

**For Opt_check4 (Combinational)**:
```
Syntax: 
read_liberty -lib <library_file_path>
read_verilog <verilog_file>
synth -top <module_name>
opt_clean -purge
abc -liberty <library_file>
show
```
**Synthesis for Opt_check4-Show**:
![opt_check4](https://github.com/sathyakanthv/VSDHDP/assets/4946509/dd8fcb37-0d6d-423a-ae4b-fc36a37896b2)

**For multiple_module_opt (Combinational)**:
```
Syntax: 
read_liberty -lib <library_file_path>
read_verilog <verilog_file>
synth -top <module_name>
flatten
opt_clean -purge
abc -liberty <library_file>
show
```
**Synthesis for  multiple_module_opt**:
![multiple_module_opt_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/1f090644-8300-42a9-8332-33c9a9613bf0)

**For multiple_module_opt2 (Combinational)**:
```
Syntax: 
read_liberty -lib <library_file_path>
read_verilog <verilog_file>
synth -top <module_name>
flatten
opt_clean -purge
abc -liberty <library_file>
show
```
**Synthesis for  multiple_module_opt2**:
![multiple_module_opt2_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/6c23d2e7-10b7-4691-8f56-10f27bdf0fa3)

**For DFF1 (Sequential)**:
```
Syntax:
iverilog <design_verilog_file.v> <tb_design_verilog_file.v>
./a.out
gtkwave <tb_design_verilog_file.vcd>
```
**Waveform for  DFF1**:
![dff_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/ec9b3afb-e016-4a21-b32c-96afd80a9277)

**Synthesis for DFF1 (Sequential)**:
```
Invoke yosys first.
Syntax:
read_liberty -lib <library_file_path>
read_verilog <design_verilog.v>
synth -top <top_module_name>
dfflibmap -liberty <library_file_path>
abc -liberty <library_file_path>
show
```
**Netlist-show for  DFF1**:
![dff1_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/1f5f95ff-2236-4af0-b712-affb8630ba02)


**For DFF2 (Sequential)**:
```
Syntax:
iverilog <design_verilog_file.v> <tb_design_verilog_file.v>
./a.out
gtkwave <tb_design_verilog_file.vcd>
```
**Waveform for  DFF2**:
![dff2_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/8ed7aa48-ceee-402a-ad76-e27d2079c985)

**Synthesis for DFF2 (Sequential)**:
```
Invoke yosys first.
Syntax:
read_liberty -lib <library_file_path>
read_verilog <design_verilog.v>
synth -top <top_module_name>
dfflibmap -liberty <library_file_path>
abc -liberty <library_file_path>
show
```
**Netlist-show for  DFF2**:
![dff2_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/abc38572-1213-4584-aad3-a933c6e801bf)

**For DFF3 (Sequential)**:
```
Syntax:
iverilog <design_verilog_file.v> <tb_design_verilog_file.v>
./a.out
gtkwave <tb_design_verilog_file.vcd>
```
**Waveform for  DFF3**:
![dff3_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/bcb66be9-3846-4991-b01c-4ec3aa9293f9)

**Synthesis for DFF3 (Sequential)**:
```
Invoke yosys first.
Syntax:
read_liberty -lib <library_file_path>
read_verilog <design_verilog.v>
synth -top <top_module_name>
dfflibmap -liberty <library_file_path>
abc -liberty <library_file_path>
show
```
**Netlist-show for  DFF3**:
![dff3_const_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/675a04d5-dec4-48e4-bca0-27b3ba877e62)

**For DFF4 (Sequential)**:
```
Syntax:
iverilog <design_verilog_file.v> <tb_design_verilog_file.v>
./a.out
gtkwave <tb_design_verilog_file.vcd>
```
**Waveform for  DFF4**:
![dff4_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/b67d2652-f554-4b35-ac43-7f6bc07fe715)

**Synthesis for DFF4 (Sequential)**:
```
Invoke yosys first.
Syntax:
read_liberty -lib <library_file_path>
read_verilog <design_verilog.v>
synth -top <top_module_name>
dfflibmap -liberty <library_file_path>
abc -liberty <library_file_path>
show
```
**Netlist-show for  DFF4**:
![dff4_const_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/b6ba2bad-1e93-49e8-9671-f833bd9c7c9a)

**For DFF5 (Sequential)**:
```
Syntax:
iverilog <design_verilog_file.v> <tb_design_verilog_file.v>
./a.out
gtkwave <tb_design_verilog_file.vcd>
```
**Waveform for  DFF5**:
![dff5_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/724d9785-7378-4f1a-a77d-1344058fa9f9)

**Synthesis for DFF5 (Sequential)**:
```
Invoke yosys first.
Syntax:
read_liberty -lib <library_file_path>
read_verilog <design_verilog.v>
synth -top <top_module_name>
dfflibmap -liberty <library_file_path>
abc -liberty <library_file_path>
show
```
**Netlist-show for  DFF5**:
![dff5_const_waveform](https://github.com/sathyakanthv/VSDHDP/assets/4946509/ba261c17-6719-4375-b478-970f9348a90e)

## Day 4: Gate Level Simulation: Blocking vs non-blocking, Synth-Simulation mismatch. 
1. GLS blocking vs non-blocking. 

Codes have been obtained from:<br>
https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git for ternary_operator_mux.v, bad_mux.v, and blocking_caveat.v <br>

**Simulation, Synthesis and GLS for the ternary operator mux**:
```
Syntax:
iverilog <ternary_operator_mux.v> <tb_ternary_operator_mux.v>
./a.out 
gtkwave tb_ternary_operator_mux.vcd
```
RTL Netlist Screenshot:
![ternary_op_mux_wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/7205c68f-5208-4da4-9e94-8402f4de4fb9)
Synth and GLS Netlist steps:
```
Syntax:
yosys> read_liberty -lib <path_of_sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <ternary_operator_mux.v>
yosys> synth -top <ternary_operator_mux>
yosys> abc -liberty <path_of_sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <ternary_operator_mux_net.v>
yosys> show
```
Synthesized design:
![ternary_op_mux_synth_show](https://github.com/sathyakanthv/VSDHDP/assets/4946509/6f5110c1-b87c-4507-88a1-6318766a17eb)

GLS Netlist:
![ternary_op_mux_netlist](https://github.com/sathyakanthv/VSDHDP/assets/4946509/5e9f9aff-9fda-4bcf-972c-187882541492)

GLS Simulation:
```
Syntax:
iverilog <verilog_model_path: ../mylib/verilog_model/primitives.v> <library_file_path: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <netlist_file: ternary_operator_mux_net.v> <Testbench_file: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
GLS netlist: <br>
1. Match and see if it matches with the original simulation of RTL Netlist.
2. It does match. 
![ternary_op_mux_glsop](https://github.com/sathyakanthv/VSDHDP/assets/4946509/b0911656-ad15-45bf-926a-edb452c6ad06)
<br>
**Simulation, Synthesis and GLS for the bad_mux**:
```
Syntax:
iverilog <bad_mux.v> <tb_bad_mux.v>
./a.out 
gtkwave tb_bad_mux.vcd
```
RTL Netlist Screenshot:
![bad_mux_wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/621e36a9-5482-43f9-b995-d425831fc436)
Synth and GLS Netlist steps:
```
Syntax:
yosys> read_liberty -lib <path_of_sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <bad_mux.v>
yosys> synth -top <bad_mux>
yosys> abc -liberty <path_of_sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <bad_mux_net.v>
yosys> show
```
Synthesized design:

GLS Netlist:
![bad_mux_netlist](https://github.com/sathyakanthv/VSDHDP/assets/4946509/ec804d41-58be-4d79-8a1f-c36fd7a86d21)

GLS Simulation:
```
Syntax:
iverilog <verilog_model_path: ../mylib/verilog_model/primitives.v> <library_file_path: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <netlist_file: bad_mux_net.v> <Testbench_file: tb_bad_mux.v>
./a.out
gtkwave tb_bad_mux.vcd
```
GLS netlist: <br>
1. Match and see if it matches with the original simulation of RTL Netlist.
2. It does not match. There is a mismatch with the RTL Simulation.
![bad_mux_wave_compare2](https://github.com/sathyakanthv/VSDHDP/assets/4946509/0d3f5fac-cc47-4ae2-88c6-8b1bcd6c6438)
<br>
**Simulation, Synthesis and GLS for the Blocking Caveat**:
```
Syntax:
iverilog <blocking_caveat.v> <tb_blocking_caveat.v>
./a.out 
gtkwave tb_blocking_caveat.vcd
```
RTL Netlist Screenshot:
![tb_blocking_caveat wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/e124502c-48d6-4297-8ca4-cecdbe1f26ee)

Synth and GLS Netlist steps:
```
Syntax:
yosys> read_liberty -lib <path_of_sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <blocking_caveat.v>
yosys> synth -top <blocking_caveat>
yosys> abc -liberty <path_of_sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <blocking_caveat_net.v>
yosys> show
```
Synthesized design:
![tb](https://github.com/sathyakanthv/VSDHDP/assets/4946509/1f2e6ac6-55ba-47eb-acd0-0741cf189ad8)

GLS Simulation:
```
Syntax:
iverilog <verilog_model_path: ../mylib/verilog_model/primitives.v> <library_file_path: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <netlist_file: blocking_caveat_net.v> <Testbench_file: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vcd
```
GLS netlist: <br>
1. Match and see if it matches with the original simulation of RTL Netlist.
2. It does not match. There is a mismatch with the RTL Simulation.
![tb_blocking_caveat wave](https://github.com/sathyakanthv/VSDHDP/assets/4946509/e227fe44-30e2-479a-8b21-60c1f834e9fe)

## Day 5: RISC V Waterlevel Detector
Objective:
1. A contactless Water level indicator is to be assessed and designed, automate the process.
2. The water level indicator is equipped with a buzzer. Functionality is that when an empty bottle is placed underneath it, the purifier will fill the bottle till the water level reaches the sensors position within the purifier.




