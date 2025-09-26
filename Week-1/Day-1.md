# Week 1 — Day 1: Introduction to Verilog RTL Design & Synthesis

**Date:** 2025-09-26  
**Repository:** [VSD-RISC-V-Tapeout](https://github.com/Vedevil/VSD-RISC-V-Tapeout)  

---

## Welcome

Welcome to **Day 1** of the RTL Workshop!  
Today, you'll begin your journey into **digital design** by working with Verilog, simulating designs using **Icarus Verilog (iverilog)**, and performing basic **logic synthesis** using **Yosys**.  

This guide will walk you through practical labs, fundamental concepts, and essential tools to help you build a strong foundation in RTL design.

---

## Table of Contents

- [Lab: Simulating a 2-to-1 Multiplexer](#lab-simulating-a-2-to-1-multiplexer)
- [Verilog Code Analysis](#verilog-code-analysis)
- [Introduction to Yosys & Gate Libraries](#introduction-to-yosys--gate-libraries)
- [Synthesis Lab with Yosys](#synthesis-lab-with-yosys)
- [Summary](#summary)
- [Next Steps](#next-steps)
- [References](#references)

---

## Lab: Simulating a 2-to-1 Multiplexer

Let's simulate a simple **2-to-1 multiplexer** using **iverilog** and visualize the results in **GTKWave**.

### **Step 1: Clone the Workshop Repository**
```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

### **Step 2: Install Required Tools**
```bash
sudo apt install iverilog
sudo apt install gtkwave
```

### **Step 3: Simulate the Design**
Compile the design and testbench:

```bash
iverilog good_mux.v tb_good_mux.v
```

Run the simulation:

```bash
./a.out
```

View the waveform in GTKWave:

```bash
gtkwave tb_good_mux.vcd
```

At this point, you should see the waveform of the multiplexer behavior in GTKWave.

---

## Verilog Code Analysis

Here's the Verilog code for the 2-to-1 Multiplexer (good_mux.v):

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

### **How It Works**
- **Inputs:**
  - `i0`, `i1` → Data inputs
  - `sel` → Select line

- **Output:**
  - `y` → Multiplexer output

- **Logic:**
  - If `sel = 1` → `y` gets `i1`
  - If `sel = 0` → `y` gets `i0`

---

## Introduction to Yosys & Gate Libraries

### **What is Yosys?**
Yosys is an open-source logic synthesis tool that converts Verilog code into a gate-level netlist.
This netlist is essentially a blueprint of the hardware that can be implemented on silicon.

**Key Yosys Features:**
- Synthesis (converts RTL to logic gates)
- Optimization (improves speed and reduces area)
- Technology mapping (maps logic to actual hardware cells)
- Verification and design rule checks
- Extensible and scriptable

### **Why Do Gate Libraries Have Different "Flavors"?**
A `.lib` file defines various types of gates, each with different characteristics:

| Property | Description |
|----------|-------------|
| Performance | Faster gates for critical paths |
| Power | Low-power gates for energy efficiency |
| Area | Smaller gates for compact designs |
| Drive Strength | Stronger gates to drive larger loads |
| Signal Integrity | Specialized gates to minimize noise |

Yosys uses these gate flavors to balance speed, area, and power during synthesis.

---

## Synthesis Lab with Yosys

Let's synthesize the multiplexer using Yosys and visualize its gate-level implementation.

### **Step-by-Step Flow**

1. **Start Yosys**
   ```bash
   yosys
   ```

2. **Read the Liberty Library**
   ```bash
   read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

3. **Read the Verilog Design**
   ```bash
   read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
   ```

4. **Run Synthesis**
   ```bash
   synth -top good_mux
   ```

5. **Technology Mapping**
   ```bash
   abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
   ```

6. **Visualize the Gate-Level Netlist**
   ```bash
   show
   ```

This will open a window showing the synthesized schematic of your design.

---

## Summary

✅ Simulated a 2-to-1 multiplexer using iverilog and visualized the waveform with GTKWave.

✅ Understood the basic Verilog code structure and functionality.

✅ Learned how Yosys performs logic synthesis and why gate libraries contain multiple flavors.

✅ Generated a gate-level netlist and visualized it using Yosys.

---

## Next Steps

- Explore synthesis reports in more detail.
- Analyze optimization techniques within Yosys.
- Move towards floorplanning concepts and layout design using Magic.

---

