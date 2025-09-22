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




