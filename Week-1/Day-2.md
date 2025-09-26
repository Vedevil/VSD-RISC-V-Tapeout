# Week 1 — Day 2: Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

**Date:** 2025-09-26  
**Repository:** [VSD-RISC-V-Tapeout](https://github.com/Vedevil/VSD-RISC-V-Tapeout)  

---

## Welcome

Welcome to **Day 2** of the RTL Workshop. This day covers three crucial topics:

1. Understanding the `.lib` timing library (`sky130_fd_sc_hd__tt_025C_1v80.lib`) used in open-source PDKs.
2. Comparing hierarchical vs. flat synthesis methods.
3. Exploring efficient coding styles for flip-flops in RTL design.

---

## Table of Contents

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
- [Summary](#summary)

---

## Timing Libraries

### SKY130 PDK Overview

The **SKY130 PDK** is an open-source Process Design Kit based on SkyWater Technology's 130nm CMOS technology. It provides essential models and libraries for integrated circuit (IC) design, including timing, power, and process variation information.

### Decoding tt_025C_1v80 in the SKY130 PDK

- **tt**: Typical process corner.
- **025C**: Represents a temperature of 25°C, relevant for temperature-dependent performance.
- **1v80**: Indicates a core voltage of 1.8V.

This naming convention clarifies which process, voltage, and temperature conditions the library models.

### Opening and Exploring the .lib File

To open the `sky130_fd_sc_hd__tt_025C_1v80.lib` file:

1. **Install a text editor:**
   ```bash
   sudo apt install gedit
   ```

2. **Open the file:**
   ```bash
   gedit sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

---

## Hierarchical vs. Flattened Synthesis

### Hierarchical Synthesis

**Definition:** Retains the module hierarchy as defined in RTL, synthesizing modules separately.

**How it Works:** Tools like Yosys process each module independently, using commands such as `hierarchy` to analyze and set up the design structure.

**Advantages:**
- Faster synthesis time for large designs.
- Improved debugging and analysis due to maintained module boundaries.
- Modular approach, aiding integration with other tools.

**Disadvantages:**
- Cross-module optimizations are limited.
- Reporting can require additional configuration.

**Example:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8dd1991a-d792-4137-acc9-78872117ab4f" />


### Flattened Synthesis

**Definition:** Merges all modules into a single flat netlist, eliminating hierarchy.

**How it Works:** The `flatten` command in Yosys collapses the hierarchy, allowing whole-design optimizations.

**Advantages:**
- Enables aggressive, cross-module optimizations.
- Results in a unified netlist, sometimes simplifying downstream processes.

**Disadvantages:**
- Longer runtime for large designs.
- Loss of hierarchy complicates debugging and reporting.
- Can increase memory usage and netlist complexity.

**Example:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/61f75db3-39ab-40c6-a928-6a2554db32a5" />


> **Important:** Hierarchical synthesis maintains sub-modules in the design, while flattening produces a netlist from the ground up.

### Key Differences

| Aspect | Hierarchical Synthesis | Flattened Synthesis |
|--------|----------------------|-------------------|
| Hierarchy | Preserved | Collapsed |
| Optimization Scope | Module-level only | Whole-design |
| Runtime | Faster for large designs | Slower for large designs |
| Debugging | Easier (traces to RTL) | Harder |
| Output Complexity | Modular structure | Single, complex netlist |
| Use Case | Modularity, analysis, reporting | Maximum optimization |

---

## Flip-Flop Coding Styles

Flip-flops are fundamental sequential elements in digital design, used to store binary data. Below are efficient coding styles for different reset/set behaviors.

### Asynchronous Reset D Flip-Flop

```verilog
module dff_asyncres (input clk, input async_reset, input d, output reg q);
  always @ (posedge clk, posedge async_reset)
    if (async_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

- **Asynchronous reset:** Overrides clock, setting `q` to 0 immediately.
- **Edge-triggered:** Captures `d` on rising clock edge if reset is low.

### Asynchronous Set D Flip-Flop

```verilog
module dff_async_set (input clk, input async_set, input d, output reg q);
  always @ (posedge clk, posedge async_set)
    if (async_set)
      q <= 1'b1;
    else
      q <= d;
endmodule
```

- **Asynchronous set:** Overrides clock, setting `q` to 1 immediately.

### Synchronous Reset D Flip-Flop

```verilog
module dff_syncres (input clk, input async_reset, input sync_reset, input d, output reg q);
  always @ (posedge clk)
    if (sync_reset)
      q <= 1'b0;
    else
      q <= d;
endmodule
```

- **Synchronous reset:** Takes effect only on the clock edge.

---

## Simulation and Synthesis Workflow

### Icarus Verilog Simulation

1. **Compile:**
   ```bash
   iverilog dff_asyncres.v tb_dff_asyncres.v
   ```

2. **Run:**
   ```bash
   ./a.out
   ```

3. **View Waveform:**
   ```bash
   gtkwave tb_dff_asyncres.vcd
   ```

### Synthesis with Yosys

1. **Start Yosys:**
   ```bash
   yosys
   ```

2. **Read Liberty library:**
   ```bash
   read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

3. **Read Verilog code:**
   ```bash
   read_verilog /path/to/dff_asyncres.v
   ```

4. **Synthesize:**
   ```bash
   synth -top dff_asyncres
   ```

5. **Map flip-flops:**
   ```bash
   dfflibmap -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

6. **Technology mapping:**
   ```bash
   abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

7. **Visualize the gate-level netlist:**
   ```bash
   show
   ```


---
## Output Windows
### GtkWave Waveform
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4fe45386-87b8-4e9a-8e54-b8427d88c3d9" />


### schemetic
<img width="1920" height="1080" alt="Screenshot from 2025-09-27 00-08-13" src="https://github.com/user-attachments/assets/a13f4577-adfc-4e32-b889-f8a61a81f38e" />



## Summary

✅ **Timing Libraries:** Understood the SKY130 PDK structure and the significance of process, voltage, and temperature conditions in library naming.

✅ **Synthesis Approaches:** Compared hierarchical vs. flattened synthesis, understanding their trade-offs in terms of optimization, runtime, and debugging.

✅ **Flip-Flop Coding:** Explored efficient coding styles for asynchronous reset, asynchronous set, and synchronous reset flip-flops.

✅ **Workflow:** Practiced complete simulation and synthesis workflow using Icarus Verilog and Yosys.

This overview provides you with practical insights into timing libraries, synthesis strategies, and reliable coding practices for flip-flops. Continue experimenting with these concepts to deepen your understanding of RTL design and synthesis.

---

## Next Steps

- Explore more complex sequential circuits and their synthesis.
- Learn about combinational logic optimization techniques.
- Study timing analysis and constraints in digital design.
