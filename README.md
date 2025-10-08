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
<a href="https://ibb.co/CKgWsC32"><img src="https://i.ibb.co/Zz5N14pm/Screenshot-from-2025-10-07-23-10-14.png" alt="Screenshot-from-2025-10-07-23-10-14" border="0"></a>
```bash
 read_verilog -sv -I /home/manohar-g/VLSI/VSDBabySoC/src/include /home/manohar-g/VLSI/VSDBabySoC/src/module/rvmyth.v
```
<a href="https://ibb.co/yn1WKtM0"><img src="https://i.ibb.co/RpmztfM2/Screenshot-from-2025-10-07-23-16-17.png" alt="Screenshot-from-2025-10-07-23-16-17" border="0"></a>
 ```bash

 read_verilog -sv -I /home/manohar-g/VLSI/VSDBabySoC/src/include /home/manohar-g/VLSI/VSDBabySoC/src/module/clk_gate.v

```
<a href="https://ibb.co/MkSwBMWH"><img src="https://i.ibb.co/B23WnqdJ/Screenshot-from-2025-10-07-23-17-10.png" alt="Screenshot-from-2025-10-07-23-17-10" border="0"></a>

---

### 2️⃣ Run Gate-Level Simulation (GLS)

```bash


vvp ../output/gls_sim.out
```

To view waveforms:

```bash
gtkwave ../output/gls_wave.vcd
```

---

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
