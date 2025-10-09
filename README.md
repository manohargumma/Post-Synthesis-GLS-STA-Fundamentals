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

## ⚙️ Setup and Hold Checks

### ⏱️ Setup Check
Ensures data arrives **before** the capturing clock edge.

**Condition**
Data Arrival Time ≤ Data Required Time

markdown
Copy code

If data arrives late → **Setup Violation** ❌  
If data arrives early → **Setup Met** ✅  

**Setup Slack**
Slack = Required Time – Arrival Time

yaml
Copy code

**Example**
| Parameter | Value |
|------------|--------|
| Clock Period | 10 ns |
| Data Arrival | 8.6 ns |
| Required Time | 10.0 ns |
| Slack | +1.4 ns (PASS ✅) |

---

### ⚡ Hold Check
Ensures data remains stable **after** the active clock edge.

**Condition**
Data Arrival Time ≥ Data Required Time

mathematica
Copy code

If data changes too soon → **Hold Violation** ❌  
If data stays stable → **Hold Met** ✅  

**Hold Slack**
Slack = Arrival Time – Required Time

yaml
Copy code

---

## 📈 Slack – The Timing Margin

| Type | Formula | Meaning |
|------|----------|----------|
| **Setup Slack** | `RT - AT` | Margin before data becomes late |
| **Hold Slack** | `AT - RT` | Margin before data changes too early |

**Positive Slack:** Timing met ✅  
**Negative Slack:** Timing violated ❌  

Slack indicates how much margin exists between data arrival and required time.

---

## 🕒 Clock Definitions in STA

The **clock** provides the reference for sequential timing checks.  
STA tools use clock definitions to determine when signals are launched and captured.

**Clock Definition (SDC Example)**
```tcl
create_clock -name core_clk -period 10 [get_ports clk]
set_clock_uncertainty 0.05 [get_clocks core_clk]
Key Clock Terms
Term	Description
Clock Period	Time between two consecutive edges
Clock Skew	Difference in arrival time of clock edges at registers
Clock Jitter	Variation of clock edge from its ideal position
Clock Uncertainty	Margin covering skew + jitter
Ideal Clock	Clock with zero delay and skew (pre-CTS assumption)

🔁 Register-to-Register (Reg2Reg) Path
scss
Copy code
      ┌────────────┐       ┌────────────┐
CLK → │  FF1 (Q)   │-----> │  FF2 (D)   │
      └────────────┘       └────────────┘
              ↑
         Combinational Logic
FF1 (Launch): Sends data on a clock edge

FF2 (Capture): Receives data on the next clock edge

Setup Check: Data must arrive before capture edge

Hold Check: Data must remain stable after launch edge

📊 Path-Based vs Graph-Based Analysis
🧠 Graph-Based Analysis (GBA)
Evaluates all paths simultaneously

Fast but pessimistic (adds unnecessary margins)

Suitable for quick design iterations

🎯 Path-Based Analysis (PBA)
Analyzes only critical paths in detail

More accurate and realistic results

Used for final timing signoff

Type	Description	Speed	Accuracy
GBA	All paths, fast, conservative	✅ High	⚠️ Pessimistic
PBA	Critical paths only	⚠️ Moderate	✅ Precise

Example

Method	Reported Slack	Result
GBA	–0.08 ns	Violation (Pessimistic)
PBA	–0.01 ns	Pass (Accurate)

⚙️ Essential STA Concepts
Concept	Description
Arrival Time (AT)	Time when data reaches endpoint
Required Time (RT)	Latest time by which data must arrive
Slew	Time for signal to transition (10%–90%)
Load	Capacitance driven by gate output
Clock-to-Q Delay	Time from clock edge to data output transition
Slack	Timing margin between RT and AT

🧮 Example OpenSTA Flow
tcl
Copy code
sta
read_liberty stdcells.lib
read_verilog synthesized_netlist.v
read_sdc constraints.sdc
update_timing
report_checks -path_delay min_max
report_timing -max_paths 10
exit
🧾 Summary
Topic	Description
Setup & Hold Checks	Ensure correct data capture timing
Slack	Defines available timing margin
Clock Definitions	Provide timing reference and uncertainty
Path-Based Analysis	Accurate timing check for critical paths
GBA vs PBA	Speed vs Precision trade-off

🧠 Prepared as part of Week 3 – Post-Synthesis GLS & STA Fundamentals Project

yaml
Copy code

---

✅ Copy this code into your repo’s `README.md` — it’s perfectly formatted, concise, and focuses on **Setup/Hold, Slack, Clock, and PBA** as required.







You’ve hit the Free plan limit for GPT-5.
You need GPT-5 to continue this chat because it has images. Your limit resets after 8:53 PM.

New chat

Upgrade to Go



