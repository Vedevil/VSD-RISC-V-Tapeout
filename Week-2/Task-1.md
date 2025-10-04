# ✅ Week 2 – Progress

## 📌 Task 1 – BabySoC Fundamentals & Functional Modelling

### 🎯 Objective

To develop a **strong foundation in SoC design concepts** and apply them through **functional modelling** of the BabySoC using **Icarus Verilog** and **GTKWave**. This step builds the base for the complete **RTL to GDSII design flow**.

---

### 📖 Theory (Conceptual Understanding)

#### 🔹 What is a System-on-Chip (SoC)?

* A **System-on-Chip (SoC)** integrates multiple components of a computing system into a single silicon chip.
* Instead of relying on separate ICs for CPU, memory, and peripherals, an SoC combines them to reduce size, cost, and power consumption.
* SoCs are at the heart of modern devices: **smartphones, IoT gadgets, automotive ECUs, AI accelerators**, and more.

Key benefits of SoCs:

* ✅ **Compactness** – Smaller form factor compared to multi-chip systems.
* ✅ **Efficiency** – Lower power consumption, optimized communication.
* ✅ **Performance** – Reduced latency due to on-chip interconnects.
* ✅ **Cost-effective** – Mass production on a single die reduces system cost.

---

#### 🔹 Components of a Typical SoC

1. **CPU (Central Processing Unit)**

   * Executes instructions and processes data.
   * Can be single-core or multi-core, general-purpose or application-specific.

2. **Memory Subsystem**

   * **ROM / Flash** → Stores firmware or boot code.
   * **RAM** → Stores temporary program and data during execution.

3. **Peripherals**

   * Interfaces for communication: **UART, SPI, I²C, GPIO, Timers, ADC/DAC**.
   * Enable interaction with external devices (sensors, displays, etc.).

4. **Interconnect / Bus**

   * Communication fabric linking CPU, memory, and peripherals.
   * Examples: **AXI, AHB, Wishbone, Avalon**.

---

#### 🔹 Why BabySoC? (Educational Importance)

* **BabySoC** is a **simplified SoC model** designed for beginners.
* Instead of overwhelming with full-fledged complexity, it focuses on **fundamentals**:

  * A minimal CPU.
  * Basic memory blocks.
  * Limited peripheral set.
* It helps students and learners **visualize SoC interactions** without requiring advanced knowledge of industry-grade architectures.
* Acts as a **stepping stone**: once you understand BabySoC, it’s easier to scale toward larger SoCs.

---

#### 🔹 Role of Functional Modelling

Before RTL synthesis or physical implementation, it’s essential to verify the **functional correctness** of the design.

* **Definition**: Functional modelling ensures that the SoC behaves as intended at the logical level.

* **Why important?**

  * Detects logic/design flaws early.
  * Ensures CPU-memory-peripheral interactions are valid.
  * Saves time and resources in later **synthesis → floorplanning → place & route → tapeout** stages.

* **Tools Used**:

  * **Icarus Verilog** → For simulation of BabySoC modules.
  * **GTKWave** → For waveform analysis, debugging signal transitions, and verifying timing relationships.

---

### 📊 Simple Block Diagram of an SoC

```
     +---------------------------+
     |         CPU Core          |
     +------------+--------------+
                  |
                  v
     +------------+--------------+
     |         Interconnect       |
     +-----+---------------+-----+
           |               |
           v               v
   +-------+----+     +----+-------+
   |   Memory   |     | Peripherals|
   | (RAM/ROM)  |     | UART, SPI, |
   |            |     | GPIO, etc. |
   +------------+     +------------+
```

This block diagram represents the **BabySoC at a conceptual level**.

---

### 📝 Deliverable

* A detailed summary of:

  * Fundamentals of **SoC design**.
  * The role and importance of **BabySoC** as a simplified model.
  * The significance of **functional modelling** in the RTL-to-GDSII design journey.
* Demonstrated understanding using **Icarus Verilog simulation** and **GTKWave waveform verification**.

**Status**: ✅ Completed
