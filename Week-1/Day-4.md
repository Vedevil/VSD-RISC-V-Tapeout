# Week 1 — Day 4: Gate-Level Simulation, Blocking vs. Non-Blocking, and Synthesis-Simulation Mismatch

**Date:** 2025-09-26  
**Repository:** [VSD-RISC-V-Tapeout](https://github.com/Vedevil/VSD-RISC-V-Tapeout)  

---

## Welcome

Welcome to **Day 4** of the RTL Workshop! Today's session focuses on three essential topics in digital design verification and coding practices:

- **Gate-Level Simulation (GLS)**
- **Blocking vs. Non-Blocking Assignments in Verilog**
- **Synthesis-Simulation Mismatch**

You'll learn both the theoretical foundations and practical implications, complete with hands-on labs to reinforce your understanding.

---

## Table of Contents

- [Gate-Level Simulation (GLS)](#gate-level-simulation-gls)
  - [Why Perform GLS?](#why-perform-gls)
  - [When is GLS Performed?](#when-is-gls-performed)
  - [Types of GLS](#types-of-gls)
- [Synthesis-Simulation Mismatch](#synthesis-simulation-mismatch)
- [Blocking vs. Non-Blocking Assignments](#blocking-vs-non-blocking-assignments)
  - [Blocking Statements](#blocking-statements)
  - [Non-Blocking Statements](#non-blocking-statements)
  - [Comparison Analysis](#comparison-analysis)
- [Practical Labs](#practical-labs)
  - [Lab 1: Ternary Operator MUX](#lab-1-ternary-operator-mux)
  - [Lab 2: Synthesis Using Yosys](#lab-2-synthesis-using-yosys)
  - [Lab 3: Gate-Level Simulation of MUX](#lab-3-gate-level-simulation-of-mux)
  - [Lab 4: Bad MUX Example](#lab-4-bad-mux-example)
  - [Lab 5: GLS of Bad MUX](#lab-5-gls-of-bad-mux)
  - [Lab 6: Blocking Assignment Caveat](#lab-6-blocking-assignment-caveat)
  - [Lab 7: Synthesis of Corrected Module](#lab-7-synthesis-of-corrected-module)
- [Summary](#summary)
- [Best Practices](#best-practices)

---

## Gate-Level Simulation (GLS)

**Gate-Level Simulation (GLS)** is a critical verification step in the VLSI design flow where the synthesized gate-level netlist of a digital circuit is simulated to validate multiple aspects of the design.

### Why Perform GLS?

GLS serves several crucial purposes in the design verification process:

- **Synthesis Validation:** Ensures that synthesis tools faithfully translate RTL code into equivalent gate-level implementation
- **Timing Verification:** Simulates with realistic propagation delays from SDF (Standard Delay Format) files to check for timing violations
- **Power Analysis:** Provides more accurate power estimates based on actual gate switching activities
- **Testability Verification:** Confirms that Design-for-Test (DFT) features like scan chains function correctly post-synthesis

### When is GLS Performed?

- **Post-Synthesis:** Immediately after RTL is converted into a gate-level netlist
- **Pre-Layout:** Before physical design phase to catch issues early in the flow
- **Post-Layout:** With extracted parasitics for final verification

### Types of GLS

| **Type** | **Description** | **Use Case** |
|----------|-----------------|--------------|
| **Functional GLS** | Logic-only simulation with zero or unit delays | Quick functional verification |
| **Timing GLS** | Uses annotated timing data for realistic delay modeling | Setup/hold time violation checking |

---

## Synthesis-Simulation Mismatch

A **synthesis-simulation mismatch** occurs when simulation results differ between RTL (pre-synthesis) and gate-level netlist (post-synthesis) implementations.

### Common Causes:

1. **Non-synthesizable Constructs:**
   - Use of delays (`#` operator)
   - Initial blocks in synthesis code
   - System tasks and functions

2. **Coding Issues:**
   - Incomplete sensitivity lists
   - Missing `else` clauses in conditional statements
   - Improper use of blocking vs. non-blocking assignments

3. **Tool Interpretation Differences:**
   - Ambiguous RTL code interpreted differently by simulation and synthesis tools
   - Race conditions in poorly written code

> **Key Point:** Always write synthesizable, unambiguous RTL following established coding guidelines to minimize mismatches.

---

## Blocking vs. Non-Blocking Assignments

Verilog provides two types of procedural assignments with fundamentally different execution behaviors.

### Blocking Statements

**Syntax:** `=` operator

**Characteristics:**
- **Execution:** Sequential, executes immediately in program order
- **Usage:** Combinational logic modeling
- **Context:** `always @(*)` or `always_comb` blocks

**Example:**
```verilog
always @(*) begin
    y = a & b;  // Executes immediately
    z = y | c;  // Uses updated value of y
end
```

### Non-Blocking Statements

**Syntax:** `<=` operator

**Characteristics:**
- **Execution:** Scheduled, all assignments execute concurrently at end of time step
- **Usage:** Sequential logic modeling (flip-flops, latches)
- **Context:** `always @(posedge clk)` or similar clocked blocks

**Example:**
```verilog
always @(posedge clk) begin
    q <= d;     // Scheduled for end of time step
    r <= q;     // Uses old value of q (creates shift register)
end
```

### Comparison Analysis

| **Aspect** | **Blocking (`=`)** | **Non-Blocking (`<=`)** |
|------------|-------------------|------------------------|
| **Execution Model** | Sequential, immediate | Concurrent, scheduled |
| **Update Timing** | Instantly in code order | At end of time step |
| **Recommended Use** | Combinational logic | Sequential logic |
| **Inference Result** | Combinational gates | Flip-flops/registers |
| **Race Conditions** | Possible with poor coding | Eliminates most races |

---

## Practical Labs

### Lab 1: Ternary Operator MUX

**Objective:** Implement a clean 2:1 multiplexer using ternary operator.

**Verilog Code:**
```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
    assign y = sel ? i1 : i0;
endmodule
```

**Functionality:**
- `y = i1` when `sel = 1`
- `y = i0` when `sel = 0`

**Lab 1 Waveform**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3a45a69-e6e5-4d26-85c5-c7bdc4675f2e" />


### Lab 2: Synthesis Using Yosys

**Objective:** Synthesize the multiplexer and examine the resulting gate-level implementation.

**Steps:**
1. Follow standard Yosys synthesis flow
2. Examine synthesized netlist
3. Verify gate count and logic optimization

**Lab 2 Schematic**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/49bea2ad-6bbd-41d7-8931-14e9d66858f9" />


### Lab 3: Gate-Level Simulation of MUX

**Objective:** Perform GLS to verify functional equivalence between RTL and gate-level implementations.

**Command:**
```bash
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```

**Verification Points:**
- Compare RTL vs. GLS waveforms
- Verify identical functional behavior
- Check timing if SDF is included

**Lab 3 Waveform (GLS)**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ff3c26c-5d3f-45b1-a552-01a1158bbfb0" />

### Lab 4: Bad MUX Example

**Objective:** Demonstrate common RTL coding mistakes and their consequences.

**Problematic Code:**
```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
    always @ (sel) begin  // Incomplete sensitivity list!
        if (sel)
            y <= i1;      // Non-blocking in combinational logic!
        else 
            y <= i0;
    end
endmodule
```

**Issues Identified:**
1. **Incomplete sensitivity list:** Missing `i0` and `i1`
2. **Wrong assignment type:** Non-blocking used for combinational logic

**Corrected Version:**
```verilog
module good_mux (input i0, input i1, input sel, output reg y);
    always @ (*) begin     // Complete sensitivity list
        if (sel)
            y = i1;        // Blocking assignment
        else
            y = i0;
    end
endmodule
```

**Lab 4 Waveform**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0d45ecea-b8d4-416e-9c3a-6d5cf2422a02" />


### Lab 5: GLS of Bad MUX

**Objective:** Observe synthesis-simulation mismatch caused by poor coding practices.

**Expected Issues:**
- Synthesis warnings about incomplete sensitivity lists
- Potential functional differences between RTL and GLS
- Incorrect inference of sequential vs. combinational logic

**Lab 5 Waveform (GLS)**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e56657fd-f92b-427f-bb3d-1a615279ff06" />


### Lab 6: Blocking Assignment Caveat

**Objective:** Understand the importance of assignment order in blocking statements.

**Problematic Code:**
```verilog
module blocking_caveat (input a, input b, input c, output reg d);
    reg x;
    always @ (*) begin
        d = x & c;    // Uses OLD value of x
        x = a | b;    // Updates x AFTER d is computed
    end
endmodule
```

**Issue:** Order dependency causes `d` to use stale value of `x`.

**Corrected Version:**
```verilog
module blocking_fixed (input a, input b, input c, output reg d);
    reg x;
    always @ (*) begin
        x = a | b;    // Compute x first
        d = x & c;    // Use updated value of x
    end
endmodule
```

**Lab 6 Waveform**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1c800fd4-faf6-4e4c-b560-2d67799886a2" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/de226df9-1aaf-456a-87f2-2241489c818f" />

### Lab 7: Synthesis of Corrected Module

**Objective:** Compare synthesis results of problematic vs. corrected blocking assignment code.

**Analysis Points:**
- Gate count differences
- Logic optimization effectiveness
- Timing characteristics
- Area implications

**Lab 7 Schematic**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/630033b4-f839-4c6a-a33e-fc53199ebec2" />

---

## Summary

✅ **Gate-Level Simulation:** Mastered the importance of GLS for verification, timing analysis, and synthesis validation.

✅ **Synthesis-Simulation Mismatch:** Identified common causes and learned prevention strategies through proper RTL coding practices.

✅ **Assignment Types:** Distinguished between blocking and non-blocking assignments, understanding their appropriate use cases and execution models.

✅ **Practical Skills:** Completed seven hands-on labs demonstrating real-world scenarios and common pitfalls in RTL design.

✅ **Debugging Techniques:** Developed skills to identify and resolve synthesis-simulation mismatches through systematic analysis.

---

## Best Practices

### RTL Coding Guidelines

- **Use appropriate assignment types:** Blocking for combinational, non-blocking for sequential
- **Complete sensitivity lists:** Use `always @(*)` for combinational logic
- **Avoid race conditions:** Proper coding eliminates most timing issues
- **Write synthesizable code:** Stick to synthesizable Verilog constructs

### Verification Strategy

- **Always perform GLS:** Verify both functional and timing behavior
- **Compare waveforms:** RTL vs. GLS should match exactly
- **Review synthesis warnings:** Address all warnings and errors
- **Use consistent coding style:** Follow established guidelines throughout project

---

## Next Steps

- Study advanced GLS techniques with SDF timing annotation
- Learn about formal verification methods
- Explore power analysis using gate-level simulations
- Practice with more complex design verification scenarios

