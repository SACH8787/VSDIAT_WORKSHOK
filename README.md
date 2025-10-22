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
![Yosys Installed](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK0/yosys_installed.png)

## Iverilog
```
$ sudo apt-get install iverilog
```
![Iverilog Installed](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK0/iverilog_installed.png)



## GTKWave
```
$ sudo apt update
$ sudo apt install gtkwave
```

![GTKWave Installed](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK0/gtkwave_installed.png)


</details>
<details>
	<summary>WEEK-1</summary>

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
![Good Mux Synthesized Netlist](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY1/good_mux_synth.png)

- View the waveform output:

$ gtkwave good_mux_tb.vcd
![Good Mux Waveform](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY1/good_mux_wave.png)

*(Ensure your testbench writes waveform data to `good_mux_tb.vcd`)*


</details>



<details>
<summary>WEEK1 - DAY 2</summary>

# WEEK1 -Day 2: Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

Welcome to Day 2 of the RTL Workshop. This day covers three crucial topics:
- Understanding the `.lib` timing library (sky130_fd_sc_hd__tt_025C_1v80.lib) used in open-source PDKs.
- Comparing hierarchical vs. flat synthesis methods.
- Exploring efficient coding styles for flip-flops in RTL design.

---


# Contents

- [Timing Libraries](#timing-libraries)
  - [SKY130 PDK Overview](#sky130-pdk-overview)
  - [Decoding tt_025C_1v80 in the SKY130 PDK](#decoding-tt_025c_1v80-in-the-sky130-pdk)
  - [Opening and Exploring the .lib File](#opening-and-exploring-the-lib-file)

- [Hierarchical vs. Flattened Synthesis](#hierarchical-vs-flattened-synthesis)
  - [Hierarchical Synthesis](#hierarchical-synthesis)
  - [Flattened Synthesis](#flattened-synthesis)
  - [Key Differences](#key-differences)

- [Flip-Flop Coding Styles](#flip-flop-coding-styles)
  - [Asynchronous Reset D Flip-Flop](#asynchronous-reset-d-flip-flop)
  - [Asynchronous Set D Flip-Flop](#asynchronous-set-d-flip-flop)
  - [Synchronous Reset D Flip-Flop](#synchronous-reset-d-flip-flop)

- [Simulation and Synthesis Workflow](#simulation-and-synthesis-workflow)
  - [Icarus Verilog Simulation](#icarus-verilog-simulation)
  - [Synthesis with Yosys](#synthesis-with-yosys)

---


## Simulation and Synthesis Workflow

### Icarus Verilog Simulation

1. **Compile:**
   ```shell
   iverilog dff_asyncres.v tb_dff_asyncres.v
   ```
2. **Run:**
   ```shell
   ./a.out
   ```
3. **View Waveform:**
   ```shell
   gtkwave tb_dff_asyncres.vcd
   ``


### Synthesis with Yosys

1. Start Yosys:
   ```shell
   yosys
   ```
2. Read Liberty library:
   ```shell
   read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
3. Read Verilog code:
   ```shell
   read_verilog /path/to/dff_asyncres.v
   ```
4. Synthesize:
   ```shell
   synth -top dff_asyncres
   ```
5. Map flip-flops:
   ```shell
   dfflibmap -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
6. Technology mapping:
   ```shell
   abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```
7. Visualize the gate-level netlist:
   ```shell
   show
   ```
![GTKWave Async Reset Simulation](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/Day2/gtkwave_asyncres.png)

![Yosys Async Reset Synthesis](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/Day2/yosys_asyncres.png)

</details>
<details>
	<summary>WEEK1- DAY3</summary>
	
## 5. Labs on Optimization

### Lab 1

Below is the Verilog code for Lab 1:

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

**Explanation:**
- `assign y = a ? b : 0;` means:
  - If `a` is true, `y` is assigned the value of `b`.
  - If `a` is false, `y` is 0.

Follow the steps from [Day 1 Synthesis Lab](https://github.com/Ahtesham18112011/RTL_workshop/tree/main/Day_1#6-synthesis-lab-with-yosys) and add the following between `abc -liberty` and `synth -top`:
```shell
opt_clean -purge
```

![Lab 1 Output](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY3/OPT_CHECK.png)

---

### Lab 2

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Code Analysis:**
- Acts as a multiplexer:
  - `y = 1` if `a` is true.
  - `y = b` if `a` is false.

![Lab 2 Output](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY3/OPT_CHECK2.png)

---

### Lab 3

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Functionality:**  
2-to-1 multiplexer; `y = a ? 1 : b` (outputs `1` when `a` is true, otherwise `b`).

![Lab 3 Output](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY3/OPT_CHECK3.png)

---

### Lab 4

Verilog code:

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```

**Functionality:**
- Three inputs (`a`, `b`, `c`), output `y`.
- Nested ternary logic:
  - If `a = 1`, `y = c`.
  - If `a = 0`, `y = !c`.
- Logic simplifies to:  
  `y = a ? c : !c`

![Lab 4 Output](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY3/OPT_CHECK4.png)

---

### Lab 5

Verilog code:

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**
- D flip-flop with:
  - Asynchronous reset to 0
  - Loads constant `1` when not in reset

![Lab 5 Output](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY3/DFF_CONST1.png)

---

### Lab 6

Verilog code:

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**
- D flip-flop always sets output `q` to `1` (regardless of reset or clock).

![Lab 6 Output](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY3/DFF_CONST2.png)

</details>

<details>
	<summary>WEEK 1 - DAY 4</summary>

 ## 4. Labs

### Lab 1: Ternary Operator MUX

Verilog code for a simple 2:1 multiplexer using a ternary operator:

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```
- **Function:** `y = i1` if `sel = 1`; else `y = i0`.



---

### Lab 2: Synthesis Using Yosys

Synthesize the above MUX using Yosys.  
_Follow the standard Yosys synthesis flow._

![lab2](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY4/ternary_operator_mux.png)

---

### Lab 3: Gate-Level Simulation (GLS) of MUX

Run GLS for the synthesized MUX.  
Use this command (adjust paths as needed):

```shell
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```



---

### Lab 4: Bad MUX Example (Common Pitfalls)

Verilog code with intentional issues:

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

#### Issues:
- **Incomplete sensitivity list**: Should include `i0`, `i1`, and `sel`.
- **Non-blocking assignment in combinational logic**: Should use blocking assignments (`=`).

**Corrected version:**
```verilog
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

![lab4](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY4/bad_mux1.png)


![lab4-1](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY4/bad_mux.png)


---

### Lab 5: GLS of Bad MUX

Perform GLS on the `bad_mux`.  
Expect simulation mismatches or warnings due to above issues.

![lab5](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY4/bad_mux2.png)

---

### Lab 6: Blocking Assignment Caveat

Verilog code:

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

#### What’s wrong?
- The order of assignments causes `d` to use the old value of `x`—not the newly computed value.
- **Best Practice:** Assign intermediate variables before using them.

**Corrected order:**
```verilog
always @ (*) begin
  x = a | b;
  d = x & c;
end
```

![lab6](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY4/blocking_caveat.png)

---

### Lab 7: Synthesis of the Blocking Caveat Module

Synthesize the corrected version of the module and observe the results.

![lab7](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK1/DAY4/blocking_caveat_tb.png)

---

## 5. Summary

- **Gate-Level Simulation (GLS):** Validates netlist functionality, timing, and testability after synthesis.
- **Synthesis-Simulation Mismatch:** Avoid by using synthesizable, unambiguous RTL code.
- **Blocking vs. Non-Blocking:** Use blocking (`=`) for combinational, non-blocking (`<=`) for sequential logic.
- **Labs:** Reinforce key concepts and highlight common RTL pitfalls.

</details>

<details>
	<SUMMARY>WEEK1 - DAY5</SUMMARY>

 Labs for If-Else and Case Statements

### Lab 1: Incomplete If Statement

```verilog
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```
![in_comp_if](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/incomp_if.png)

---

### Lab 2: Synthesis Result of Lab 1

![incomp_synth](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/incomp_if_tb.png)

---

###Lab 3: Nested If-Else

```verilog
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```
![icomp2](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/incomp_if2.png)

---

### Lab 4: Synthesis Result of Lab 3

![incomp2synth](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/incomp_if2_tb.png)

---

### Lab 5: Complete Case Statement

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```
![compcase](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/comp_case.png)

---

### Lab 6: Synthesis Result of Lab 5

![compcase_synth](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/comp_case_tb.png)

---

## 7. Labs on Loops and Generate Blocks

### Lab 9: 4-to-1 MUX Using For Loop

```verilog
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```
![mux_generate](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/mux_generate_tb.png)

---

### Lab 10: 8-to-1 Demux Using Case

```verilog
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```
![demux-case](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/demux_case_tab.png)

---

8-bit Ripple Carry Adder with Generate Block

```verilog
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```
**Full Adder Module:**
```verilog
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```
![rca_org](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/WEEK-1/WEEK1/DAY5/rca_tb.png)

---
</details>
</details>


<details>
	<summary>WEEK-2</summary>
# Introduction to the VSDBabySoC

VSDBabySoC is a small yet powerful RISCV-based SoC. The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. VSDBabySoC contains one RVMYTH microprocessor, an 8x-PLL to generate a stable clock, and a 10-bit DAC to communicate with other analog devices.

  ![vsdbabysoc_block_diagram](images/vsdbabysoc_block_diagram.png)

## Problem statement

This work discusses the different aspects of designing a small SoC based on RVMYTH (a RISCV-based processor). This SoC will leverage a PLL as its clock generator and controller and a 10-bit DAC as a way to talk to the outside world. Other electrical devices with proper analog input like televisions, and mobile phones could manipulate DAC output and provide users with music sound or video frames. At the end of the day, it is possible to use this small fully open-source and well-documented SoC which has been fabricated under Sky130 technology, for educational purposes.

## What is SoC

An SoC is a single-die chip that has some different IP cores on it. These IPs could vary from microprocessors (completely digital) to 5G broadband modems (completely analog).

## What is RVMYTH

RVMYTH core is a simple RISCV-based CPU, introduced in a workshop by RedwoodEDA and VSD. During a 5-day workshop students (including middle-schoolers) managed to create a processor from scratch. The workshop used the TLV for faster development. All of the present and future contributions to the IP will be done by students and under open-source licenses.

## What is PLL

A phase-locked loop or PLL is a control system that generates an output signal whose phase is related to the phase of an input signal. PLLs are widely used for synchronization purposes, including clock generation and distribution.

## What is DAC

A digital-to-analog converter or DAC is a system that converts a digital signal into an analog signal. DACs are widely used in modern communication systems enabling the generation of digitally-defined transmission signals. As a result, high-speed DACs are used for mobile communications and ultra-high-speed DACs are employed in optical communications systems.

# VSDBabySoC Modeling

Here we are going to model and simulate the VSDBabySoC using `iverilog`, then we will show the results using `gtkwave` tool. Some initial input signals will be fed into `vsdbabysoc` module that make the pll start generating the proper `CLK` for the circuit. The clock signal will make the `rvmyth` to execute instructions in its `imem`. As a result the register `r17` will be filled with some values cycle by cycle. These values are used by dac core to provide the final output signal named `OUT`. So we have 3 main elements (IP cores) and a wrapper as an SoC and of-course there would be also a testbench module out there.

Please note that in the following sections we will mention some repos that we used to model the SoC. However the main source code is resided in [Source-Code Directory](src) and these modules are in [Modules Sub-Directory](src/module).

## RVMYTH modeling

As we mentioned in [What is RVMYTH](#what-is-rvmyth) section, RVMYTH is designed and created by the TL-Verilog language. So we need a way for compile and trasform it to the Verilog language and use the result in our SoC. Here the `sandpiper-saas` could help us do the job.

  [Here](https://github.com/shivanishah269/risc-v-core) is the repo we used as a reference to model the RVMYTH

## PLL and DAC modeling

It is not possible to sythesis an analog design with Verilog, yet. But there is a chance to simulate it using `real` datatype. We will use the following repositories to model the PLL and DAC cores:

  1. [Here](https://github.com/vsdip/rvmyth_avsdpll_interface) is the repo we used as a reference to model the PLL
  2. [Here](https://github.com/vsdip/rvmyth_avsddac_interface) is the repo we used as a reference to model the DAC

**CAUTION:** In the beginning of the project, we get our verilog model of the PLL from [here](https://github.com/vsdip/rvmyth_avsdpll_interface). However, by proceeding the project to the physical design flow we realize that this model needs a little changes to become sufficient for a real IP core. So we changed it a little and created a new model named `AVSDPLL` based on [this](https://github.com/lakshmi-sathi/avsdpll_1v8) IP

## Step by step modeling walkthrough

In this section we will walk through the whole process of modeling the VSDBabySoC in details. We will increase/decrease the digital output value and feed it to the DAC model so we can watch the changes on the SoC output. Please, note that the following commands are tested on the Ubuntu Bionic (18.04.5) platform and no other OSes.

  1. First we need to install some important packages:

  ```
  $ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io
  $ sudo chmod 666 /var/run/docker.sock
  $ cd ~
  $ pip3 install pyyaml click sandpiper-saas
  ```

  2. Now we can clone this repository in an arbitrary directory (we'll choose home directory here):

  ```
  $ cd ~
  $ git clone https://github.com/manili/VSDBabySoC.git
  ```

  3. It's time to make the `pre_synth_sim.vcd`:

  ```
  $ cd VSDBabySoC
  $ make pre_synth_sim
  ```
  
  The result of the simulation (i.e. `pre_synth_sim.vcd`) will be stored in the `output/pre_synth_sim` directory.

  4. We can see the waveforms by following command:

  ```
  $ gtkwave output/pre_synth_sim/pre_synth_sim.vcd
  ```
  
  Two most important signals are `CLK` and `OUT`. The `CLK` signal is provided by the PLL and the `OUT` is the output of the DAC model. Here is the final result of the modeling process:
  
  ![pre_synth_sim](images/pre_synth_sim.png)

In this picture we can see the following signals:

  * **CLK:** This is the `input CLK` signal of the `RVMYTH` core. This signal comes from the PLL, originally.
  * **reset:** This is the `input reset` signal of the `RVMYTH` core. This signal comes from an external source, originally.
  * **OUT:** This is the `output OUT` signal of the `VSDBabySoC` module. This signal comes from the DAC (due to simulation restrictions it behaves like a digital signal which is incorrect), originally.
  * **RV_TO_DAC[9:0]:** This is the 10-bit `output [9:0] OUT` port of the `RVMYTH` core. This port comes from the RVMYTH register #17, originally.
  * **OUT:** This is a `real` datatype wire which can simulate analog values. It is the `output wire real OUT` signal of the `DAC` module. This signal comes from the DAC, originally.

**PLEASE NOTE** that the sythesis process does not support `real` variables, so we must use the simple `wire` datatype for the `\vsdbabysoc.OUT` instead. The `iverilog` simulator always behaves `wire` as a digital signal. As a result we can not see the analog output via `\vsdbabysoc.OUT` port and we need to use `\dac.OUT` (which is a `real` datatype) instead.

# OpenLANE

OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The main usage of OpenLANE in this project is for [VSDBabySoC Physical Design](#vsdbabysoc-physical-design). However, we need OpenLANE for the synthesis and STA process in the [Post-synthesis simulation](#post-synthesis-simulation) section. So we'll talk about its installation process here and let the details be until the [VSDBabySoC Physical Design](#vsdbabysoc-physical-design) section.

## OpenLANE installation

The OpenLANE and sky130 installation can be done by following the steps in this repository `https://github.com/nickson-jose/openlane_build_script`.

* More information on OpenLANE can be found in the following repositories:

  * `https://github.com/The-OpenROAD-Project/OpenLane`
  * `https://github.com/efabless/openlane`

To summerize the installation processes:

  ```
  $ git clone https://github.com/The-OpenROAD-Project/OpenLane.git
  $ cd OpenLane/
  $ make openlane
  $ make pdk
  $ make test
  ```

For more info please refer to the GitHub repositories.

**PLEASE NOTE** that currently we are using commit version `8580c248a995b575f7734813b80bb6c4aa82d4f2` for the OpenLANE and our docker image version is `2021.09.09_03.00.48`.

# Post-synthesis simulation

First step in the design flow is to synthesize the generated RTL code and after that we will simulate the result. This way we can find more about our code and its bugs. So in this section we are going to synthesize our code then do a post-synthesis simulation to look for any issues. The post and pre (modeling section) synthesis results should be identical.

## Synthesizing using Yosys

* In OpenLANE the RTL synthesis is performed by `yosys`.
* The technology mapping is performed by `abc`.
* Finally, the timing reports for the synthesized netlist are generated by `OpenSTA`.

## How to synthesize the design

To perform the synthesis process do the following:

  ```
  $ cd ~/VSDBabySoC
  $ make synth
  ```

The heavy job will be done by the script. When the process has been done, we can see the result in the `output/synth/vsdbabysoc.synth.v` file.

## Post-synthesis simulation (GLS)

There is an issue for post-synthesis simulation (Gate-Level Simulation) which can be tracked [here](https://github.com/google/skywater-pdk/issues/310). However, we hacked the source-code by the following instructions and we managed to workaround the issue for now ([here is the reference](https://github.com/The-OpenROAD-Project/OpenLane/issues/518)):

  1. In `$YOUR_PDK_PATH/sky130A/libs.ref/sky130_fd_sc_hd/verilog/sky130_fd_sc_hd.v` file we should manually correct `endif SKY130_FD_SC_HD__LPFLOW_BLEEDER_FUNCTIONAL_V` to `endif //SKY130_FD_SC_HD__LPFLOW_BLEEDER_FUNCTIONAL_V`.
  2. We can simulate with the functional models by passing the `FUNCTIONAL` define to `iverilog`. Also we need to set `UNIT_DELAY` macro to some value. As a result we'll have `iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 <THE SOURCE-CODEs TO BE COMPILED>`.

User could bypass these confusing steps by using our provided Makefile:

  ```
  $ cd ~/VSDBabySoC
  $ make post_synth_sim
  ```
The result of the simulation (i.e. `post_synth_sim.vcd`) will be stored in the `output/post_synth_sim` directory and the waveform could be seen by the following command:

  ```
  $ gtkwave output/post_synth_sim/post_synth_sim.vcd
  ```
Here is the final result:

  ![post_synth_sim](images/post_synth_sim.png)

In this picture we can see the following signals:

  * **\core.CLK:** This is the `input CLK` signal of the `RVMYTH` core. This signal comes from the PLL, originally.
  * **reset:** This is the `input reset` signal of the `RVMYTH` core. This signal comes from an external source, originally.
  * **OUT:** This is the `output OUT` signal of the `VSDBabySoC` module. This signal comes from the DAC (due to simulation restrictions it behaves like a digital signal which is incorrect), originally.
  * **\core.OUT[9:0]:** This is the 10-bit `output [9:0] OUT` port of the `RVMYTH` core. This port comes from the RVMYTH register #17, originally.
  * **OUT:** This is a `real` datatype wire which can simulate analog values. It is the `output wire real OUT` signal of the `DAC` module. This signal comes from the DAC, originally.

**PLEASE NOTE** that the sythesis process does not support `real` variables, so we must use the simple `wire` datatype for the `\vsdbabysoc.OUT` instead. The `iverilog` simulator always behaves `wire` as a digital signal. As a result we can not see the analog output via `\vsdbabysoc.OUT` port and we need to use `\dac.OUT` (which is a `real` datatype) instead.



VSDBabySoC-Simulation/
│
├── README.md                  # Simulation report (pre-filled)
├── simulation_logs/           # Terminal logs from simulation
│   ├── pre_synth_sim.log
│   ├── vvp_sim.out
│   └── synth.log (optional)
│
├── screenshots/               # GTKWave waveform screenshots
│   ├── reset_waveform.png
│   ├── clk_waveform.png
│   ├── fetch_decode.png
│   ├── alu_waveform.png
│   └── mem_waveform.png
│
├── src/                       # Original source files
│   ├── module/
│   │   ├── rvmyth.tlv
│   │   └── testbench.v
│   └── include/
│       └── *.vh or header files
│
├── output/                    # Auto-generated files (add to .gitignore)
│   ├── pre_synth_sim/
│   ├── compiled_tlv/
│   └── synth/
│
└── Makefile                   # Build/simulation commands



2. Observations
2.1 Reset Operation


Observation:
When reset = 1, the program counter (pc) and all pipeline registers are cleared to zero. After reset is deasserted (reset = 0), the CPU begins fetching instructions sequentially.

2.2 Clocking


Observation:
The CPU state updates on the rising edge of clk. The program counter increments on each cycle, demonstrating synchronous operation of the BabySoC pipeline.

2.3 Instruction Fetch & Decode


Observation:
Instruction memory outputs (instr) reflect the instruction being fetched at the current pc. The decoded signals (rd, rs1, rs2) are ready for ALU execution in the following cycle.

2.4 ALU Operation


Observation:
The ALU executes arithmetic and logical instructions using operands from the register file. The result (alu_out) is written back to the register file or memory depending on the instruction type.

2.5 Memory Operations


Observation:
Load and store instructions trigger memory read/write signals (dmem_rd_en, dmem_wr_en). Memory addresses and data values are updated accordingly.

3. Simulation Logs

Simulation logs are saved in the simulation_logs/ folder:

pre_synth_sim.log → pre-synthesis simulation output

vvp_sim.out → simulation execution output

synth.log → synthesis logs (optional if using OpenLane)

Example terminal command to view logs:

cat simulation_logs/pre_synth_sim.log

4. Conclusion

The BabySoC CPU demonstrates correct operation:

Proper reset and clock synchronization

Accurate instruction fetch, decode, and execution

Correct ALU computation and memory access

Waveforms confirm that all modules are communicating as expected, and the TL-Verilog design has been successfully converted and simulated.

![img1](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK2/SOC_DESIGN_FLOW.png)
![img2](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK2/Screenshot%20from%202025-09-28%2019-21-05.png)
![img3](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK2/Screenshot%20from%202025-09-28%2019-25-49.png)



</details>
<details>
	<summary>WEEK -3 </summary>
	# BabySoC GLS & Post-Synthesis Simulation

## **Gate-Level Simulation (GLS) of BabySoC**

### **Purpose of GLS**
Gate-Level Simulation is used to verify the functionality of a design **after the synthesis process**. Unlike RTL (Register Transfer Level) simulations, which work at a higher abstraction level, GLS works on the **actual synthesized netlist**, including gates and interconnections.  

#### **Key Aspects of GLS for BabySoC**

- **Verification with Timing Information**:  
  GLS can include Standard Delay Format (SDF) files to verify timing correctness under realistic conditions.  

- **Design Validation Post-Synthesis**:  
  Ensures the design’s logical behavior remains correct after mapping to gate-level cells.  
  Detects issues like glitches or metastability.  

- **Simulation Tools**:  
  - **Icarus Verilog** for compiling and simulating netlists  
  - **GTKWave** for waveform visualization  

- **Importance for BabySoC**:  
  BabySoC has multiple modules such as the RISC-V processor, PLL, and DAC. GLS ensures correct interactions between these modules after synthesis.  

---

## **Step-by-Step Execution Plan**

### **Step 1: Load the Top-Level Design and Supporting Modules**


yosys
Inside the Yosys shell, run:

tcl
Copy code
read_verilog /home/sachin-mohanty/VSDBabySoC/src/module/vsdbabysoc.v
read_verilog -I /home/sachin-mohanty/VSDBabySoC/src/include /home/sachin-mohanty/VSDBabySoC/src/module/rvmyth.v
read_verilog -I /home/sachin-mohanty/VSDBabySoC/src/include /home/sachin-mohanty/VSDBabySoC/src/module/clk_gate.v

Step 2: Load the Liberty Files for Synthesis
tcl
Copy code
read_liberty -lib /home/sachin-mohanty/VSDBabySoC/src/lib/avsdpll.lib
read_liberty -lib /home/sachin-mohanty/VSDBabySoC/src/lib/avsddac.lib
read_liberty -lib /home/sachin-mohanty/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

Step 3: Run Synthesis Targeting vsdbabysoc
tcl
Copy code
synth -top vsdbabysoc

Step 4: Map D Flip-Flops to Standard Cells
tcl
Copy code
dfflibmap -liberty /home/sachin-mohanty/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

Step 5: Perform Optimization and Technology Mapping
tcl
Copy code
opt
abc -liberty /home/sachin-mohanty/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}

Step 6: Perform Final Clean-Up and Renaming
tcl
Copy code
flatten
setundef -zero
clean -purge
rename -enumerate

Step 7: Check Statistics
tcl
Copy code
stat

Step 8: Write the Synthesized Netlist
tcl
Copy code
write_verilog -noattr /home/sachin-mohanty/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v

Post-Synthesis Simulation and Waveforms
Step 1: Compile the Testbench
bash
Copy code
iverilog -o /home/sachin-mohanty/VSDBabySoC/output/post_synth_sim/post_synth_sim.out \
-DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 \
-I /home/sachin-mohanty/VSDBabySoC/src/include \
-I /home/sachin-mohanty/VSDBabySoC/src/module \
/home/sachin-mohanty/VSDBabySoC/src/module/testbench.v
Step 2: Navigate to Post-Synthesis Simulation Directory
bash
Copy code
cd /home/sachin-mohanty/VSDBabySoC/output/post_synth_sim/
Step 3: Run the Simulation
bash
Copy code
./post_synth_sim.out
Step 4: View the Waveforms in GTKWave
bash
Copy code
gtkwave post_synth_sim.vcd
![img1](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/1.png)
![img2](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/2.png)
![img3](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/3.png)
![img4](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/4.png)
![img5](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/5.png)
![img6](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/6.png)
![img7](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/7.png)
![img8](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK3/8.png)



# Static Timing Analysis (STA) – BabySoC

## **Purpose**
- STA is used to **verify timing correctness** of a digital design **without dynamic simulation**.
- Ensures all signals propagate correctly through **combinational and sequential logic** within the clock period.
- Detects **setup and hold violations** for flip-flops and other sequential elements.

---

## **Key Concepts**
- **Clock Period (Tclk):** Maximum allowed time for signals to propagate before the next clock edge.
- **Setup Time:** Minimum time a data signal must be stable **before** the clock edge.
- **Hold Time:** Minimum time a data signal must remain stable **after** the clock edge.
- **Slack:** Difference between the required arrival time and actual arrival time at a flip-flop or latch.
  - **Positive Slack:** Timing is met.
  - **Negative Slack:** Timing violation exists.

---

## **Components Analyzed**
- **Combinational Paths:** Logic between flip-flops or I/O pins.
- **Sequential Elements:** Flip-flops, latches, registers.
- **Clock Paths:** Clock distribution network including delays, skew, and jitter.

---

## **Workflow for STA**

### **1. Prepare Netlist**
- Use the **post-synthesis netlist**, e.g., `vsdbabysoc.synth.v` mapped to standard cells.

### **2. Define Timing Constraints**
- Provide an **SDC (Synopsys Design Constraints)** file with:
  - Clock definitions
  - Input/output delays
  - Setup/hold timing constraints

### **3. Run STA Tool**
- Example command (inside OpenLane docker):

sta -exit -threads max /VSDBabySoC/src/script/sta.conf | tee ../output/sta/sta.log
</details>


















<details>
	<summary>WEEK-4</summary>

	I HAVE EXCUTED ALL THE .SPICE FILES AND SAVED THE .RAW FILES IN RESULTS
![I](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D1.png)
	
---

## Experiments

### 1. MOSFET Behavior – Id vs Vds
**Netlist:** `nmos_id_vds.spice`  
**Plot:** `id_vds.png`  

**Theory:**  
- The drain current \(I_D\) of an NMOS transistor depends on both **Vgs** (gate-source voltage) and **Vds** (drain-source voltage).  
- For **Vds < Vgs - Vt**, the transistor operates in the **linear (ohmic) region**; current increases linearly with Vds.  
- For **Vds ≥ Vgs - Vt**, the transistor enters **saturation**, where current becomes almost constant.

**What the plot shows:**  
- Multiple Id vs Vds curves for different Vgs values.  
- Early curves rise linearly (linear region) then plateau (saturation).  
- Demonstrates how increasing Vgs increases saturation current.

![Id vs Vds](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D21.png)

---

### 2. Threshold Voltage Extraction – Id vs Vgs
**Netlist:** `nmos_id_vgs.spice`  
**Plot:** `id_vgs_sqrt.png`  

**Theory:**  
- The threshold voltage \(V_t\) is the minimum Vgs at which the MOSFET turns ON.  
- Using the **square-root method**: in saturation, \(I_D \propto (V_{GS}-V_t)^2\).  
- Plotting \(\sqrt{I_D}\) vs Vgs allows **linear extrapolation to find Vt**.

**What the plot shows:**  
- Linear sections of \(\sqrt{I_D}\) vs Vgs.  
- Extrapolated line intersects x-axis at Vt (threshold voltage).  
- Important for understanding device turn-on and timing constraints.

![Id vs Vgs](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D22.png)
![Id vs Vgs](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D31.png)

---

### 3. CMOS Inverter – Voltage Transfer Characteristic (VTC)
**Netlist:** `inv_vtc.spice`  
**Plot:** `vtc_vdd1.6_1.8_2.0.png`  

**Theory:**  
- A CMOS inverter switches an input high (Vdd) to low (0 V) at the output, and vice versa.  
- **VTC** plots Vout vs Vin; the **switching threshold Vm** is where Vin = Vout.  
- Noise margins are calculated as:
  - \(V_{IL}, V_{IH}\) (input low/high voltage limits)  
  - \(V_{OL}, V_{OH}\) (output low/high voltage)  
  - \(NM_L = V_{IL}-V_{OL}, NM_H = V_{OH}-V_{IH}\)

**What the plot shows:**  
- VTC curves for VDD = 1.6 V, 1.8 V, 2.0 V.  
- Vm shifts slightly with supply voltage.  
- The slope at Vm indicates gain; steeper slope → faster transition.

![VTC at different VDD](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D32.png)
![VTC at different VDD](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D322.png)

---

### 4. CMOS Inverter – Transient Response
**Netlist:** `inv_transient.spice`  
**Plot:** `transient_waveforms.png`  

**Theory:**  
- Apply a pulse input to the inverter to observe **rise and fall propagation delays**.  
- **t_rise**: time for output to go from 10% → 90% of VDD.  
- **t_fall**: time for output to go from 90% → 10% of VDD.

**What the plot shows:**  
- Input pulse vs output waveform.  
- Time difference at VDD/2 crossing gives **propagation delay**.  
- Rise/fall delays are slightly asymmetric due to PMOS/NMOS sizing.

![Transient waveform](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D4.png)

---

### 5. CMOS Inverter – Device & Supply Variations
**Netlist:** `inv_variation.spice`  
**Plot:** (included in VTC plots or separate)  

**Theory:**  
- Changing **W/L ratios** or **supply voltage** affects:  
  - Switching threshold Vm  
  - Noise margins  
  - Rise/fall delays  
- Demonstrates **robustness** of the inverter to manufacturing or environmental variations.

**What the plot shows:**  
- Overlay of VTC curves for different W/L and VDD values.  
- Vm shifts and slope changes are visible, illustrating sensitivity of CMOS circuits.
![Transient waveform2](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK4/D5.png)
---

## References
- SkyWater PDK: [https://skywater-pdk.readthedocs.io](https://skywater-pdk.readthedocs.io)  
- Neil H.E. Weste, David Harris, *CMOS VLSI Design*, 4th Edition  
- Ngspice Documentation: [http://ngspice.sourceforge.net/docs.html](http://ngspice.sourceforge.net/docs.html)

---

This README gives a **high-level overview** of each experiment, the underlying **theory**, and a visual **reference to the plots** in your `plots/` folder.  

---

If you want, I can also **write a ready-to-use Jupyter Notebook version** of this README, where all `.raw` files are loaded and **plots are generated automatically** — so you don’t have to manually plot each one in ngspice. That makes it **fully automated and reproducible**.  

Do you want me to do that?

</details>

















<details>
	<summary>WEEK-5</summary>
	# VSD Hardware Design Program

## OpenROAD installation guide

### 📚 Contents

  - [Steps to Install OpenROAD and Run GUI](#steps-to-install-openroad-and-run-gui)
    - [1. Clone the OpenROAD Repository](#1-clone-the-openroad-repository)
    - [2. Run the Setup Script](#2-run-the-setup-script)
    - [3. Build OpenROAD](#3-build-openroad)
    - [4. Verify Installation](#4-verify-installation)
    - [5. Run the OpenROAD Flow](#5-run-the-openroad-flow)
    - [6. Launch the GUI](#6-launch-the-graphical-user-interface-gui-to-visualize-the-final-layout)
- [ORFS Directory Structure and File Formats](#orfs-directory-structure-and-file-formats)


**OpenROAD** is an open-source, fully automated RTL-to-GDSII flow for digital integrated circuit (IC) design. It supports synthesis, floorplanning, placement, clock tree synthesis, routing, and final layout generation. OpenROAD enables rapid design iterations, making it ideal for academic research and industry prototyping.

### `Steps to Install OpenROAD and Run GUI`

### 1. Clone the OpenROAD Repository

```bash
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```

### 2. Run the Setup Script

```bash
sudo ./setup.sh
```
![Alt Text](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK5/Screenshot%20from%202025-10-22%2023-03-47.png)

### 3. Build OpenROAD

```bash
./build_openroad.sh --local
```




### 4. Verify Installation

```bash
source ./env.sh
yosys -help  
openroad -help
```


### 5. Run the OpenROAD Flow

```bash
cd flow
make
```

[![Alt Text](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK5/Screenshot%20from%202025-10-22%2023-04-16.png)
### 6. Launch the graphical user interface (GUI) to visualize the final layout

```bash
 make gui_final
```

[![Alt Text](https://github.com/SACH8787/VSDIAT_WORKSHOK/blob/main/WEEK5/Screenshot%20from%202025-10-23%2000-03-54.png)

✅ Installation Complete! You can now explore the full RTL-to-GDSII flow using OpenROAD.

### `ORFS Directory Structure and File formats`

OpenROAD-flow-scripts/

```plaintext
├── OpenROAD-flow-scripts             
│   ├── docker           -> It has Docker based installation, run scripts and all saved here
│   ├── docs             -> Documentation for OpenROAD or its flow scripts.  
│   ├── flow             -> Files related to run RTL to GDS flow  
|   ├── jenkins          -> It contains the regression test designed for each build update
│   ├── tools            -> It contains all the required tools to run RTL to GDS flow
│   ├── etc              -> Has the dependency installer script and other things
│   ├── setup_env.sh     -> Its the source file to source all our OpenROAD rules to run the RTL to GDS flow
```
Inside the `flow/` Directory

```plaintext
├── flow           
│   ├── design           -> It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── makefile         -> The automated flow runs through makefile setup
│   ├── platform         -> It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts                 
```

![Alt Text](Images/installation7.jpg)
</details>
