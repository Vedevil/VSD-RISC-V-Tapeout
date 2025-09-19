# VSD-RISC-V-Tapeout
This repository will keep track of the weekly progress of the RISC-V Tapeout program.

# RTL to GDSII Journey

## ✅ Week 0 – Progress

| Task   |              Description             |     Status     |
|--------|--------------------------------------|----------------|
| Task 0 | Install and verify open-source tools | ✅ Completed   |
| Task 1 | Summarize the video on SoC design flow | ✅ Completed   |

---

## 📝 Week 0 - Task 1 Summary

The Week 0 Task 1 objective is to **understand and summarize the SoC design flow** explained by **Kunal Ghosh** in the introductory video.  
This lays the foundation for the RTL to GDSII journey.

---

### **Overview of Stages (O0 to O4)**

1. **O0 – High-Level C Model**  
   - Initial design using **C language** and **GCC**.  
   - A simple testbench is created in C to validate basic functionality.  
   - Purpose: Verify algorithms before hardware implementation.

2. **O1 – Specification**  
   - Define complete functional specifications.  
   - Inputs, outputs, and architecture are finalized.  
   - Testing still uses C environment.

3. **O2 – RTL Development**  
   - Specification is converted to **synthesizable Verilog RTL**.  
   - Components designed include:
     - Processor Core  
     - Peripherals and IPs  
     - Analog IP Models (for integration)
   - RTL is verified using the testbench from O1.

4. **O3 – Synthesis and SoC Integration**  
   - Convert RTL to **Gate-Level Netlist** using synthesis tools.  
   - Integrate all components into a complete SoC.  
   - Physical design steps:
     - Floorplanning
     - Placement
     - Clock Tree Synthesis (CTS)
     - Routing
     - **DRC (Design Rule Check)** and **LVS (Layout vs. Schematic)**.

5. **O4 – Final GDSII and Fabrication**  
   - Final design is exported as a **GDSII file** and sent for fabrication.  
   - Typical applications:
     - Smartwatches
     - Arduino boards
     - TV panels
     - AC controllers  
   - Target frequency: **100 MHz – 130 MHz**.

---

### **Overall Flow**
