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



```bash
dfflibmap -liberty /home/manohar-g/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

```
![image alt](
### 5️⃣ Perform Optimization and Technology Mapping
```bash

$opt
```
![image alt](
```bash
abc -liberty /home/manohar-g/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}

```
![image alt](
### 6️⃣ Perform Final Clean-Up and Renaming

```bash

flatten
```

```bash
setundef -zero
```
```bash
 clean -purge
```
```bash
 rename -enumerate

```
### 7️⃣ Check Statistics

```bash
stat


```
### 8️⃣ Write the Synthesized Netlist
```bash
iverilog -o /home/manohar-g/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out \ -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 \ -I /home/manohar-g/VLSI/VSDBabySoC/src/include \ -I /home/manohar-g/VLSI/VSDBabySoC/src/module \ /home/manohar-g/VLSI/VSDBabySoC/src/module/testbench.v
```
## 🧾 Results Summary

| Stage          | Simulator | Output             | Result        |
| -------------- | --------- | ------------------ | ------------- |
| RTL Simulation | iverilog  | pre_synth_sim.vcd  | ✅ Correct     |
| GLS Simulation | iverilog  | post_synth_sim.vcd | ✅ Matches RTL |
| Synthesis      | yosys     | top_netlist.v      | ✅ Completed   |

---

## 🔍 Observations

* GLS exposes **uninitialized signals** and **timing issues** not visible in RTL simulations.
* After adding proper reset logic, all outputs matched between RTL and GLS.
* Delay annotation (`UNIT_DELAY=#1`) helped confirm timing stability.

---

## 🧠 Lessons Learned

* Always include a **global reset** to avoid ‘X’ states in GLS.
* Check for **latches** or **floating nets** in synthesis reports.
* Use **formal equivalence (yosys -equiv)** to complement GLS verification.

---

## 📁 Repository Structure

```
VSDBabySoC_GLS/
├── src/
│   ├── core/       # RISC-V core (rvmyth)
│   ├── pll/        # PLL module
│   ├── dac/        # DAC module
│   ├── include/    # Header files
│   └── module/     # SoC integration modules 
├── synth/
│   ├── yosys/
│   │   └── synth.ys
│   └── netlist/
│       └── top_netlist.v
├── sim/
│   ├── tb/
│   ├── scripts/
│   └── output/
├── docs/
│   └── GLS_flow.md
├── LICENSE
└── README.md
```

---

## 🧩 Next Steps

* Add **SDF Back-Annotation** for realistic delay modeling.
* Integrate **Formal Equivalence** verification.
* Automate GLS runs via **Makefile** or **GitHub Actions** CI.

---

## 🙌 Contributors

* **Manohar Gumma** – Design, synthesis, and GLS implementation
* **VSD Flow / SKY130 Community** – Open-source ecosystem

---

## 🪪 License

This project is licensed under the [MIT License](LICENSE).

---
