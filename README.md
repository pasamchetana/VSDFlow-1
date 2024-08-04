# VSDFlow
<details>
## Day 0 - System/Tool Setup Check. GitHub ID creation

### Install Yosys

#### Yosys is a Verilog HDL synthesis tool. This means that it takes a behavioural design description as input and generates an RTL, logical gate or physical gate level description of the design as output. Yosys’ main strengths are behavioural and RTL synthesis.

$ sudo apt-get update

$ git clone https://github.com/YosysHQ/yosys.git

$ cd yosys

$ sudo apt install make (If make is not installed ,install it)

$ sudo apt-get install build-essential clang bison flex
libreadline-dev gawk tcl-dev libffi-dev git
graphviz xdot pkg-config python3 libboost-system-dev
libboost-python-dev libboost-filesystem-dev zlib1g-dev

$ make config-gcc

$ make

$ sudo make install

<img width="429" alt="image" src="https://github.com/user-attachments/assets/56ee140f-3948-4352-a7ec-4787843e3890">


### install iverilog

#### iverilog: Icarus Verilog, commonly known as Iverilog, is an open-source tool used for the simulation and synthesis of digital circuits described in Verilog hardware description language (HDL).Primarily, Iverilog is used to simulate Verilog designs, allowing designers to verify the functionality of their digital circuits before physical implementation.

$ sudo apt-get install iverilog

<img width="397" alt="image" src="https://github.com/user-attachments/assets/4c3d4240-1a52-474b-8b3b-1af524da6aa6">

### install gtkwave

sudo apt-get update

sudo apt install gtkwave

<img width="574" alt="image" src="https://github.com/user-attachments/assets/7edf13db-e248-4153-af7e-5e47627cb532">

</details>

## <Day 1 - Introduction to Verilog RTL Design and Synthesis>

In RTL design, the adherence to specifications is verified through simulation using a tool called a simulator. For this course, the simulator used is Icarus Verilog (Iverilog). The design refers to the actual Verilog code or set of codes that implement the intended functionality to meet the required specifications.

A testbench is used to apply stimulus, or test vectors, to the design to check its functionality. The simulator operates by monitoring changes in input signals; when an input changes, the corresponding output is evaluated. If there are no changes in the input, the output remains unchanged, as the simulator continuously checks for variations in input values to determine the resultant outputs.

### Block diagram of Test Bench

<img width="571" alt="image" src="https://github.com/user-attachments/assets/1e1b9aea-2c98-47b7-9dea-2d35718766ce">

### iverilog based simulation flow

![image](https://github.com/user-attachments/assets/31d49bf1-6689-48fa-8f9f-e5a159bdaaf8)

### LAB1- good_mux

Run the simulation on good_mux with the test bench tb_good_mux in iverilog.

<img width="364" alt="image" src="https://github.com/user-attachments/assets/2deb5f15-cc0e-4351-81d0-c53efeff4c18">

### testbench for good_mux

<img width="613" alt="image" src="https://github.com/user-attachments/assets/1cbc1109-4900-4262-8440-d453bb17ef69">

Icarus Verilog (Iverilog) takes design files and testbench files as inputs. The output of Iverilog is a .vcd (Value Change Dump) file, an ASCII-based format generated by EDA logic simulation tools. This file records changes in signal values throughout the simulation. The .vcd file can then be used as an input for the GTKWave tool, which visualizes the waveforms of the design's signals. By viewing these waveforms, one can verify the correctness of signal transitions according to the stimulus provided by the testbench.

#### Commands used

The output will be a.out file for the simulation 

<img width="608" alt="image" src="https://github.com/user-attachments/assets/c958b4f3-442e-4fc6-83a9-cc4454bd914c">

To view the simulation results, use a waveform viewer like GTKWave. Open the GTKWave and load the generated VCD file:

### Waveforms in gtkwave

<img width="604" alt="image" src="https://github.com/user-attachments/assets/efac9690-d4ea-48fb-80e3-b7e30b7cc0e3">

## Introduction to Yosys RTL Synthesizer

Yosys is an open-source framework for Verilog RTL synthesis. It is used primarily for converting high-level Verilog descriptions of digital circuits into gate-level netlists that can be implemented on FPGAs or used for ASIC design. Yosys is highly versatile and supports various front-end and back-end tools, making it a valuable tool for digital design and synthesis.

Inputs for Yosys tool: Design (.v), Liberty (.lib)
Output: Netlist file (.net.v)

<img width="600" alt="image" src="https://github.com/user-attachments/assets/574fea39-d6d6-46f2-9495-1867525ad5ed">

Verifying the design:

Use the Test bench using in RTL phase.Using the same stimulus used for RTL, the output expected should be the same as the one obtained in RTL phase.

<img width="598" alt="image" src="https://github.com/user-attachments/assets/db8d6336-823e-4225-a51f-3f95918fbb4b">

### Synthesis

The process of transforming the RTL description into a lower-level representation consisting of gates and their interconnections. This process is typically performed using a synthesis tool, which maps the RTL code to a specific technology library containing various gate-level components like AND, OR, NOT gates, flip-flops, and more.

<img width="184" alt="image" src="https://github.com/user-attachments/assets/d255aaa7-104f-42f2-92d1-dc63f8ca1012">

### What is .lib?

A .lib file, or a standard cell library file, is a collection of different standard cells that vary in functionality, input configurations, and other characteristics. These cells can implement various logic functions and are available in multiple performance models, such as slow, medium, and fast. Each model caters to different design requirements, providing options for trade-offs in speed, power, and area. This library enables designers to choose the appropriate cells to meet specific design criteria and constraints.

Block diagram shows how the Synthesizer works

<img width="440" alt="image" src="https://github.com/user-attachments/assets/a0eda8cc-2df0-4c11-a8e8-26d19454299c">

### Synthesis of good_mux.v

#### 1.Read the liberty source file

yosys> read_liberty -lib ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

#### 2. Read the Verilog source file

yosys> read_verilog good_mux.v

#### 3. Do Synthesis

yosys> synth -top good_mux

<img width="608" alt="image" src="https://github.com/user-attachments/assets/f1b30a9a-6b51-41e1-8675-c01fa1f12f26">

<img width="611" alt="image" src="https://github.com/user-attachments/assets/9b5b32b9-ad4c-43d5-a88f-abf5c1da890a">

### Technology Mapping to the Design using the abc tool which is integrated with Yosys:

yosys> abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

<img width="605" alt="image" src="https://github.com/user-attachments/assets/d414464c-58bf-44cc-82ba-faa1c8590762">

<img width="605" alt="image" src="https://github.com/user-attachments/assets/e7f45182-7529-4667-a9c6-9078a8ca64fe">

### Generated Gate level Netlist

yosys> show

<img width="604" alt="image" src="https://github.com/user-attachments/assets/66b32437-262d-46a3-a918-71128b05d529">


















