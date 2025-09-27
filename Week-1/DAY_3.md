# Week 1 — Day 3: Combinational and Sequential Optimization

**Date:** 2025-09-26  
**Repository:** [VSD-RISC-V-Tapeout](https://github.com/Vedevil/VSD-RISC-V-Tapeout)  

---

## Welcome

Welcome to **Day 3** of this workshop! Today we discuss optimization of combinational and sequential circuits, introducing techniques to enhance efficiency and performance in digital design.

---

## Table of Contents

- [Optimization Techniques](#optimization-techniques)
  - [Constant Propagation](#constant-propagation)
  - [State Optimization](#state-optimization)
  - [Cloning](#cloning)
  - [Retiming](#retiming)
- [Practical Labs](#practical-labs)
  - [Lab 1: Simple Conditional Logic](#lab-1-simple-conditional-logic)
  - [Lab 2: Multiplexer Logic](#lab-2-multiplexer-logic)
  - [Lab 3: Complex Ternary Logic](#lab-3-complex-ternary-logic)
  - [Lab 4: Nested Conditional Logic](#lab-4-nested-conditional-logic)
  - [Lab 5: Constant Loading Flip-Flop](#lab-5-constant-loading-flip-flop)
  - [Lab 6: Always High Flip-Flop](#lab-6-always-high-flip-flop)
- [Summary](#summary)
- [Next Steps](#next-steps)

---

## Optimization Techniques

### Constant Propagation

In VLSI design, **constant propagation** is a compiler optimization technique used to replace variables with their constant values during synthesis. This can significantly simplify design and enhance performance.

#### **How It Works:**
Constant propagation analyzes the design code to identify variables with constant values. These are replaced directly, allowing synthesis tools to simplify logic and reduce circuit size.

#### **Benefits:**
- **Reduced Complexity:** Simpler logic equations result in smaller circuits
- **Performance Improvement:** Faster execution with reduced propagation delays
- **Resource Optimization:** Fewer gates or flip-flops required for implementation

*Constant Propagation Example*

### State Optimization

**State optimization** refines finite state machines (FSMs) to improve efficiency in IC design. It reduces the number of states, optimizes encoding, and minimizes logic complexity.

#### **How It's Done:**
1. **State Reduction:** Merge equivalent states using minimization algorithms
2. **State Encoding:** Assign optimal binary codes to states for efficient implementation
3. **Logic Minimization:** Apply Boolean algebra or automated tools for compact equations
4. **Power Optimization:** Implement techniques like clock gating to reduce dynamic power

### Cloning

**Cloning** duplicates a logic cell or module to optimize performance, reduce power consumption, or improve timing characteristics by balancing loads or reducing wire lengths.

#### **Implementation Steps:**
1. Identify critical paths using static timing analysis tools
2. Duplicate the target cell or module
3. Redistribute connections to balance electrical loads
4. Optimize placement and routing for the cloned elements
5. Verify improvements through timing and power analysis

*Cloning Example*

### Retiming

**Retiming** is a design optimization technique that improves circuit performance by repositioning registers (flip-flops) without changing the circuit's functionality.

#### **Process:**
1. **Graph Representation:** Model the circuit as a directed graph with weights
2. **Register Repositioning:** Move registers to balance critical path delays
3. **Constraints Analysis:** Maintain timing requirements and functional equivalence
4. **Optimization:** Adjust register positions to minimize clock period and optimize power

---

## Practical Labs

### Lab 1: Simple Conditional Logic

**Verilog Code:**
```verilog
module opt_check (input a, input b, output y);
    assign y = a ? b : 0;
endmodule
```

**Functionality:**
- `assign y = a ? b : 0;` implements a conditional assignment:
  - If `a` is true (1), `y` is assigned the value of `b`
  - If `a` is false (0), `y` is assigned 0

**Synthesis Commands:**
Follow the steps from Day 1 Synthesis Lab and add the following between `abc -liberty` and `synth -top`:
```bash
opt_clean -purge
```

**Lab 1 Output:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/26720f7a-230a-47c8-b2fe-11b504d7891e" />


### Lab 2: Multiplexer Logic

**Verilog Code:**
```verilog
module opt_check2 (input a, input b, output y);
    assign y = a ? 1 : b;
endmodule
```

**Code Analysis:**
Acts as a 2-to-1 multiplexer:
- `y = 1` when `a` is true
- `y = b` when `a` is false

**Lab 2 Output:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8821dcd-c982-4011-af6e-410325ac5cd9" />


### Lab 3: Complex Ternary Logic

**Verilog Code:**
```verilog
module opt_check3 (input a, input b, input c, output y);
    assign y = a ? (c ? b : 0) : 0;
endmodule
```

**Functionality:**
- Implements nested conditional logic with three inputs
- Output `y` depends on both `a` and `c` conditions
- Synthesizer can optimize this to simpler gate-level implementation

**Lab 3 Output:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/49f2c788-b64e-46af-8a54-c5bbbcedf84e" />


### Lab 4: Nested Conditional Logic

**Verilog Code:**
```verilog
module opt_check4 (input a, input b, input c, output y);
    assign y = a ? (b ? (a & c) : c) : (!c);
endmodule
```

**Functionality:**
- Three inputs (`a`, `b`, `c`) with complex nested ternary logic
- The logic can be simplified through Boolean algebra:
  - If `a = 1`, then `y = c`
  - If `a = 0`, then `y = !c`
- Simplified expression: `y = a ? c : !c`

**Lab 4 Output:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f78dda87-c53d-4bf1-ad7a-c59c71ed1880" />


### Lab 5: Constant Loading Flip-Flop

**Verilog Code:**
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
- D flip-flop with asynchronous reset
- **Reset behavior:** Sets `q` to 0 when `reset` is asserted
- **Normal operation:** Loads constant 1 when not in reset
- This represents a sequential circuit that can be optimized

**Lab 5 Output:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/84fb5366-9b3d-4e31-af58-f5d110bc9e1d" />


### Lab 6: Always High Flip-Flop

**Verilog Code:**
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
- D flip-flop that **always** sets output `q` to 1
- Regardless of reset or clock state, output remains constant
- Synthesis tool can optimize this to a simple constant driver

**Lab 6 Output:**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cddf70cc-2127-4c2b-8168-8897468576b0" />

---

## Summary

✅ **Optimization Techniques:** Explored four key optimization methods for digital circuit design:
- Constant propagation for logic simplification
- State optimization for FSM efficiency
- Cloning for performance and timing improvements
- Retiming for balanced pipeline design

✅ **Practical Implementation:** Completed six hands-on Verilog labs demonstrating:
- Combinational logic optimizations with conditional assignments
- Sequential circuit behavior with constant loading patterns
- Real-world synthesis optimization using Yosys tools

✅ **Analysis Skills:** Developed ability to:
- Identify optimization opportunities in RTL code
- Understand how synthesis tools simplify complex logic
- Recognize patterns that lead to efficient hardware implementation

---

## Next Steps

- Study advanced optimization techniques like resource sharing and scheduling
- Explore timing-driven optimization strategies
- Learn about power optimization methods in digital design
- Practice with more complex FSM optimization examples
