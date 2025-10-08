### Week-3


# GLS OF BABYSOC
## Post-Synthesis GLS  
## Purpose of GLS

The Gate-Level Simulation (GLS) verifies the correctness of a synthesized netlist by ensuring it behaves exactly like the original RTL design.

When a circuit is synthesized, its Verilog code is converted into a gate-level representation (composed of standard cells like AND, OR, flip-flops, etc.).
GLS helps confirm that this transformation did not change the intended behavior.
# 🧠 VSDBabySoC – Gate-Level Simulation (GLS)

![Toolchain-Yosys](https://img.shields.io/badge/Tool-Yosys-blue)
![Simulator-Icarus\_Verilog](https://img.shields.io/badge/Simulator-Icarus_Verilog-orange)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🎯 Purpose of GLS

The **Gate-Level Simulation (GLS)** verifies the correctness of a **synthesized netlist** by ensuring it behaves exactly like the original **RTL design**.

When a circuit is synthesized, its Verilog code is converted into a **gate-level representation** (composed of standard cells like AND, OR, flip-flops, etc.).
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

> 💡 *In simple terms:*
> **GLS ensures that what you built (in RTL) is what you’ll actually get (in silicon).**

---

## ⚙️ Tools Used

| Tool                    | Purpose                                 | Example Version |
| ----------------------- | --------------------------------------- | --------------- |
| **Yosys**               | Synthesis (generate gate-level netlist) | 0.57+           |
| **Icarus Verilog**      | Simulation (pre and post-synthesis)     | 11.0            |
| **GTKWave**             | View waveform results                   | —               |
| **OpenLane (optional)** | PnR using SKY130 PDK                    | —               |

---

## 🧩 Flow Overview

1. **RTL Simulation** → Verify functional correctness.
2. **Synthesis (Yosys)** → Generate `netlist.v`.
3. **GLS Simulation (Icarus Verilog)** → Run simulation using gate-level netlist.
4. **Compare Waveforms** → Confirm GLS matches RTL behavior.

---

## 🧠 Step-by-Step Commands

### 1️⃣ Run Synthesis (Yosys)

```bash
yosys -s synth/yosys/synth.ys
```

**Example synth.ys**

```tcl
read_verilog -sv ../../src/module/top.v ../../src/core/*.v ../../src/pll/*.v ../../src/dac/*.v
flatten
synth -top top
write_verilog ../../synth/netlist/top_netlist.v
```

---

### 2️⃣ Run Gate-Level Simulation (GLS)

```bash
cd sim/scripts
iverilog -o ../output/gls_sim.out \
  -I ../../src/include -I ../../src/module \
  -DFUNCTIONAL -DUNIT_DELAY=#1 \
  ../../synth/netlist/top_netlist.v ../../sim/tb/top_tb.v

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
