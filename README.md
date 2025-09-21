# 	VSDIAT_WORKSHOK
<details>
	<summary>WEEK 0 - Tools Installation </summary>
	
# Day 0 - Tools Installation
## Yosys
```
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys 
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make 
$ sudo make install
```
<img width="575" alt="yosys" src="[https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/7dfb067d-5f8c-407b-86eb-6bcb44f60a97](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK0/yosys_installed.png)">

## Iverilog
```
$ sudo apt-get install iverilog
```
<img width="702" alt="iverilog" src="[https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e660c9fc-0d6d-4ab7-b75f-9992133771ef](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK0/iverilog_installed.png)">

## GTKWave
```
$ sudo apt update
$ sudo apt install gtkwave
```
<img width="604" alt="gtkwave2" src="[https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/843a73bc-20ec-4417-bdc8-883caa6a299b](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK0/gtkwave_installed.png)">

</details>

<details>
    <summary>WEEK 1 - Day 0 - Simulation and Synthesis Steps</summary>
    
# Week 1 - Day 0 - Simulation and Synthesis

## Yosys - Synthesis and Netlist Diagram
$ yosys
yosys> read_verilog verilog/good_mux.v
yosys> synth -top good_mux
yosys> show

- `read_verilog verilog/good_mux.v` reads your Verilog file.
- `synth -top good_mux` synthesizes your top module named `good_mux`.
- `show` opens a GTK window displaying the synthesized netlist diagram.

## Icarus Verilog - Simulation and Waveform Viewing

- Compile your Verilog source and testbench:

$ iverilog -o good_mux_tb.vvp verilog/good_mux.v testbench/good_mux_tb.v


- Run the simulation:

$ vvp good_mux_tb.vvp


- View the waveform output:

$ gtkwave good_mux_tb.vcd


*(Ensure your testbench writes waveform data to `good_mux_tb.vcd`)*


</details>



