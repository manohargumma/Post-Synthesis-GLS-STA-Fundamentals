### Week-3


# GLS OF BABYSOC

![Toolchain-Yosys](https://img.shields.io/badge/Tool-Yosys-blue)
![Simulator-Icarus\_Verilog](https://img.shields.io/badge/Simulator-Icarus_Verilog-orange)



## Post-Synthesis GLS  
## Purpose of GLS

The Gate-Level Simulation (GLS) verifies the correctness of a synthesized netlist by ensuring it behaves exactly like the original RTL design.

When a circuit is synthesized, its Verilog code is converted into a gate-level representation (composed of standard cells like AND, OR, flip-flops, etc.).
GLS helps confirm that this transformation did not change the intended behavior.


### ✅ Key Objectives

1. **Functional Verification After Synthesis**

   * Ensures the gate-level netlist produces the same results as RTL for all test cases.
2. **Timing Validation**

   * Verifies that the circuit performs correctly even when realistic gate delays are introduced.
3. **Reset & Initialization Check**

   * Detects uninitialized registers or flip-flops that could lead to ‘X’ (unknown) values during simulation.
4. **Optimization Integrity Check**

   * Confirms that synthesis optimizations did not alter or remove required logic.
5. **Confidence Before Tapeout**

   * Validates that the synthesized hardware behaves as intended before physical design and fabrication.

## ⚙️ Tools Used

| Tool                    | Purpose                                 | Example Version |
| ----------------------- | --------------------------------------- | --------------- |
| **Yosys**               | Synthesis (generate gate-level netlist) | 0.57+           |
| **Icarus Verilog**      | Simulation (pre and post-synthesis)     | 11.0            |
| **GTKWave**             | View waveform results                   | —               |
| **OpenLane (optional)** | PnR using SKY130 PDK                    | —               |


## 🧠 Step-by-Step Commands 

### 1️⃣ Read Verilog Source Files

```bash
 read_verilog /home/manohar-g/VLSI/VSDBabySoC/src/module/vsdbabysoc.v

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-10-14.png)
```bash
 read_verilog -sv -I /home/manohar-g/VLSI/VSDBabySoC/src/include /home/manohar-g/VLSI/VSDBabySoC/src/module/rvmyth.v
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-16-17.png)
 ```bash

 read_verilog -sv -I /home/manohar-g/VLSI/VSDBabySoC/src/include /home/manohar-g/VLSI/VSDBabySoC/src/module/clk_gate.v

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-17-10.png)



### 2️⃣ Read Liberty Files
```bash

read_liberty -lib /home/manohar-g/VLSI/VSDBabySoC/src/lib/avsdpll.lib

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-21-27.png)

```bash
read_liberty -lib /home/manohar-g/VLSI/VSDBabySoC/src/lib/avsddac.lib
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-22-17.png)
```bash
read_liberty -lib /home/manohar-g/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-24-39.png)
### 3️⃣ Run Synthesis
```bash
synth -top vsdbabysoc

```


<p align="center">
  <img src="https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2308bbea4a3d7b51fa37e70d7267ba2fbf03e443/Screenshot%20from%202025-10-07%2023-27-16.png" alt="Yosys synthesis setup" width="80%">
</p>


<p align="center">
  <img src="https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/main/Screenshot%20from%202025-10-07%2023-28-04.png" alt="Synthesis progress" width="80%">
</p>


<p align="center">
  <img src="https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2308bbea4a3d7b51fa37e70d7267ba2fbf03e443/Screenshot%20from%202025-10-07%2023-28-31.png" alt="Netlist generation" width="75%">
</p>


<p align="center">
  <img src="https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2308bbea4a3d7b51fa37e70d7267ba2fbf03e443/Screenshot%20from%202025-10-07%2023-28-48.png" alt="GLS iverilog run" width="75%">
</p>


<p align="center">
  <img src="https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2308bbea4a3d7b51fa37e70d7267ba2fbf03e443/Screenshot%20from%202025-10-07%2023-29-26.png" alt="Post-synth simulation output" width="70%">
</p>


<p align="center">
  <img src="https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2308bbea4a3d7b51fa37e70d7267ba2fbf03e443/Screenshot%20from%202025-10-07%2023-29-37.png" alt="GLS verification complete" width="70%">
</p>

### 4️⃣ Map D Flip-Flops to Standard Cells

```bash
dfflibmap -liberty /home/manohar-g/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/d19c73a011980fa99840aeda6236368bf577bee7/Screenshot%20from%202025-10-07%2023-32-03.png)
### 5️⃣ Perform Optimization and Technology Mapping
```bash

$opt
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2d770bf0f4969b860d5d56e1c2777f2a65da34a9/Screenshot%20from%202025-10-07%2023-32-25.png)
```bash
abc -liberty /home/manohar-g/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/2d770bf0f4969b860d5d56e1c2777f2a65da34a9/Screenshot%20from%202025-10-07%2023-34-07.png)
### 6️⃣ Perform Final Clean-Up and Renaming

```bash

flatten
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c53ac320b067cf9feda530fa758ecba2144563a7/Screenshot%20from%202025-10-07%2023-34-48.png)

```bash
setundef -zero
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c53ac320b067cf9feda530fa758ecba2144563a7/Screenshot%20from%202025-10-07%2023-35-15.png)
```bash
 clean -purge
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/1df693beaf0f37367cb27a36cbf1a022b82c2001/Screenshot%20from%202025-10-07%2023-35-40.png)

```bash
 rename -enumerate

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c53ac320b067cf9feda530fa758ecba2144563a7/Screenshot%20from%202025-10-07%2023-36-34.png)
### 7️⃣ Check Statistics

```bash
stat


```

![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/7c3a334eaa734eb84950d499d893a205dd40387e/Screenshot%20from%202025-10-07%2023-37-20.png)
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/7c3a334eaa734eb84950d499d893a205dd40387e/Screenshot%20from%202025-10-07%2023-37-48.png)
### 8️⃣ Write the Synthesized Netlist
```bash
write_verilog -noattr output/synth/vsdbabysoc.synth.v
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/7c3a334eaa734eb84950d499d893a205dd40387e/Screenshot%20from%202025-10-07%2023-45-33.png)



# 🧩 Post-Synthesis Simulation and Waveforms

This section provides a step-by-step guide to perform **post-synthesis simulation** and **view waveforms** for the **VSDBabySoC** project.

---

## ⚙️ Step 1: Compile the Testbench

Use the following `iverilog` command to compile the post-synthesis testbench:

```bash
iverilog -o  /home/manohar-g/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I /home/manohar-g/VLSI/VSDBabySoC/src/include -I /home/manohar-g/VLSI/VSDBabySoC/src/module home/manohar-g/VLSI/VSDBabySoC/src/module/testbench.v

```

### 📘 Explanation:

* **`iverilog`** – Compiles the Verilog files.
* **`-o`** – Specifies the output executable file.
* **`-DPOST_SYNTH_SIM`** – Defines a macro for post-synthesis simulation.
* **`-DFUNCTIONAL`** – Enables functional simulation mode.
* **`-DUNIT_DELAY=#1`** – Adds a unit delay for gate-level simulation.
* **`-I`** – Includes the necessary source directories.
* **`testbench.v`** – The top-level testbench file for simulation.

---
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c24ce908348f2d59b1e1bd43c3403c2307f9a1b5/Screenshot%20from%202025-10-07%2023-55-33.png)
##  Step 2: Navigate to the Output Directory

Change the directory to where the post-synthesis simulation output is stored:

```bash
cd output/post_synth_sim/
```


## ▶️ Step 3: Run the Simulation

Execute the compiled simulation file:

```bash
./post_synth_sim.out
```

![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c24ce908348f2d59b1e1bd43c3403c2307f9a1b5/Screenshot%20from%202025-10-07%2023-55-45.png)


## 📊 Step 4: View Waveforms Using GTKWave

Open the generated waveform file with GTKWave to visualize signal activity:

```bash
gtkwave post_synth_sim.vcd
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c24ce908348f2d59b1e1bd43c3403c2307f9a1b5/Screenshot%20from%202025-10-07%2023-56-29.png)
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/c24ce908348f2d59b1e1bd43c3403c2307f9a1b5/Screenshot%20from%202025-10-08%2000-04-01.png)



## 🧾 Results Summary

| Stage          | Simulator | Output             | Result        |
| -------------- | --------- | ------------------ | ------------- |
| RTL Simulation | iverilog  | pre_synth_sim.vcd  | ✅ Correct     |
| GLS Simulation | iverilog  | post_synth_sim.vcd | ✅ Matches RTL |
| Synthesis      | yosys     | top_netlist.v      | ✅ Completed   |



---

# 🧮 Static Timing Analysis (STA) – Week 3 Task

## 🎯 Objective
To understand and perform **Static Timing Analysis (STA)** on a synthesized netlist using **OpenSTA**, focusing on:
- Setup and Hold checks  
- Slack calculation  
- Clock definitions  
- Path-Based Analysis (PBA)

---

## 📘 What is STA?

**Static Timing Analysis (STA)** is used to verify the timing of a digital circuit **without simulation**.  
It analyzes all possible signal paths to ensure data is launched and captured correctly between sequential elements.

STA ensures:
- Correct data transfer between registers  
- No setup or hold time violations  
- The design meets its clock frequency target  

---


## ⏱️ Timing Cheacks 

### Arrival Timing
 is the moment when a signal reaches a specific point in a digital circuit, like the input of a flip-flop or gate. It is a key metric in **Static Timing Analysis (STA)** used to ensure the design meets timing requirements.  

### Required Timing
 is the time by which a signal must arrive at a specific point in a digital circuit to meet timing constraints. It ensures that data is correctly captured at flip-flops or correctly processed through combinational logic.  

### Slack

**Slack** is the difference between the required arrival time and the actual arrival time of a signal at a point in a digital circuit. It indicates whether timing constraints are met.  

- **Slack = Required Time − Arrival Time**  
- **Interpretation:**  
  - Positive slack → timing met (signal arrives on time)  
  - Negative slack → timing violation (signal arrives late)  

### Setup and Hold Analysis

**Setup and Hold Analysis** checks if data signals meet timing requirements around flip-flops to ensure correct operation. It ensures that data is stable **before** and **after** the clock edge.  

#### 1. **Setup Time Analysis**
- **Definition:** Checks if data arrives **early enough** before the clock edge.  
- **Purpose:** Ensures flip-flops capture correct data.  
- **Violation:** Data arrives too late → setup violation.  

#### 2. **Hold Time Analysis**
- **Definition:** Checks if data remains **stable after** the clock edge.  
- **Purpose:** Prevents data from changing too soon after the clock triggers.  
- **Violation:** Data changes too early → hold violation.  


### Types of Timing Analysis Paths in STA

In **Static Timing Analysis (STA)**, various paths are analyzed to ensure timing correctness in digital circuits:  

#### 1. **Register-to-Register (Reg-to-Reg)**
- Timing from one flip-flop to another.  
- Checks setup and hold requirements along the combinational path.  

#### 2. **Input-to-Register (In2Reg)**
- Timing from external inputs to internal flip-flops.  
- Ensures input data arrives in time to meet setup/hold constraints.  

#### 3. **Register-to-Output (Reg2Out)**
- Timing from internal flip-flops to output pins.  
- Ensures outputs meet required arrival times.  

#### 4. **Input-to-Output (In2Out)**
- Timing from external inputs directly to outputs through combinational logic.  
- Checks purely combinational delays.  

#### 5. **Clock Gating Analysis**
- Checks timing of gated clocks to ensure proper enable/disable without glitches.  

#### 6. **Recovery and Removal Checks**
- **Recovery:** Minimum time after an asynchronous reset before a flip-flop can safely capture data.  
- **Removal:** Maximum time after a reset before flip-flop must capture valid data.  

#### 7. **Data-to-Data (D2D) Paths**
- Timing between two data signals in combinational paths, used in latch-based designs.  

#### 8. **Latch Time-Borrowing Paths**
- Latches can allow "time borrowing" across clock phases.  
- STA checks that data can safely use part of the next clock phase without violating setup/hold.  

### Slew Analysis

**Slew Analysis** checks the **rate of change of a signal** (rise and fall time) as it propagates through gates and interconnects. It ensures signals transition fast enough for reliable operation.  

- **Slew Rate:** Time taken for a signal to transition from low → high (rise) or high → low (fall).  
- **Types of Slew in STA:**  
  1. **Data Slew (Max/Min):**  
     - **Max data slew:** Slowest transition, can cause setup violations.  
     - **Min data slew:** Fastest transition, can cause hold violations or glitches.  
  2. **Clock Slew (Max/Min):**  
     - **Max clock slew:** Slow clock edges may reduce timing margin, causing setup issues.  
     - **Min clock slew:** Very fast edges may increase overshoot or crosstalk, affecting hold.  

- **Purpose:**  
  - Ensures signals meet **timing constraints**.  
  - Prevents **timing violations, glitches, and signal integrity issues**.  

### Load Analysis

**Load Analysis** evaluates the **capacitive load** driven by gates and interconnects in a digital circuit. It ensures that signals propagate fast enough to meet timing constraints.  

- **Definition:** Total load seen by a gate output due to connected gates, interconnects, and pins.  
- **Key Components:**  
  1. **Fanout:** Number of gates driven by a single output.  
     - Higher fanout → higher delay → potential setup violations.  
  2. **Capacitance:** Sum of input capacitances of connected gates + wire capacitance.  
     - Higher capacitance → slower signal transitions → affects both setup and hold.  
### Clock Analysis

**Clock Analysis** checks key properties of the clock signal to ensure reliable operation of sequential circuits.  

- **Clock Skew:**  
  - The difference in arrival time of the clock at different flip-flops.  
  - Positive skew reduces setup margin; negative skew reduces hold margin.  

- **Clock Width:**  
  - The duration of the clock pulse (high or low phase).  
  - Ensures flip-flops have enough time to capture and propagate data correctly.
   
# 💻 Static Timing Analysis LAB – Week 3 Task
## Static timing analysis using OpenSTA

### Timing Ananlysis Using In line Commands
```bash
cd /VLSI/OpenSTA/build
./sta
read_liberty /home/manohar-g/VLSI/OpenSTA/examples/nangate45_fast.lib
read_verilog /home/manohar-g/VLSI/OpenSTA/example/example1.v
link_design top
create_clock -name clk -period 10 {clk1 clk2 clk3}
set_input_delay -clock clk 0 {in1 in2}
report_checks
```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/ab6836667bcd3cbf53197e4df1f605205eff8250/Screenshot%20from%202025-10-09%2018-24-54.png)
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/a77725a8b5d4ad15e2c64f46cd88333ebbf1f4f9/Screenshot%20from%202025-10-09%2018-24-13.png)

### Min Max Timming Analysis using IN line Commands
```bash
read_liberty -min /home/manohar-g/VLSI/OpenSTA/examples/nangate45_fast.lib
read_verilog -min /home/manohar-g/VLSI/OpenSTA/example/example1.v

read_liberty -max /home/manohar-g/VLSI/OpenSTA/examples/nangate45_fast.lib
read_verilog -max /home/manohar-g/VLSI/OpenSTA/example/example1.v
link_design top
create_clock -name clk -period 10 {clk1 clk2 clk3}
set_input_delay -clock clk 0 {in1 in2}
report_checks

```
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/a77725a8b5d4ad15e2c64f46cd88333ebbf1f4f9/Screenshot%20from%202025-10-11%2011-49-50.png)
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/a77725a8b5d4ad15e2c64f46cd88333ebbf1f4f9/Screenshot%20from%202025-10-11%2011-49-58.png)
![image alt](https://github.com/manohargumma/Post-Synthesis-GLS-STA-Fundamentals/blob/a77725a8b5d4ad15e2c64f46cd88333ebbf1f4f9/Screenshot%20from%202025-10-11%2011-50-05.png)
