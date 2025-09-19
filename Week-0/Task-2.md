### 1️⃣ Yosys – RTL Synthesis
**Purpose:** Converts RTL code into gate-level representations for synthesis and optimization.

**Installation:**
```bash
$ git clone https://github.com/YosysHQ/yosys.git  
$ cd yosys  
$ sudo apt install build-essential clang bison flex libreadline-dev gawk \
    tcl-dev libffi-dev git graphviz xdot pkg-config python3 \
    libboost-system-dev libboost-python-dev libboost-filesystem-dev zlib1g-dev  
$ make  
$ sudo make install  
```
**Verification:**  
```bash
$ yosys -V
```  
<img width="917" height="771" alt="Screenshot from 2025-09-19 23-31-26" src="https://github.com/user-attachments/assets/3d6b6e22-c9a1-4ff3-a74f-b791f1a2b7ca" />

✅ Yosys installed successfully


---


### 2️⃣ Icarus Verilog – Verilog Simulation
**Purpose:** Compiles and simulates Verilog designs for functional verification.

**Installation:**  
```bash
$ sudo apt-get install iverilog  
```
**Verification:**  
```bash
$ iverilog -V  
```
<img width="785" height="309" alt="image" src="https://github.com/user-attachments/assets/277bd30d-6948-456e-8a33-bdcede74a98a" />

✅ Icarus Verilog installed successfully


---


### 3️⃣ GTKWave – Waveform Viewer
**Purpose:** Analyzes and visualizes simulation waveforms for debugging.

**Installation:**  
```bash
$ sudo apt update  
$ sudo apt install gtkwave  
```
**Verification:**  
```bash
$ gtkwave  
```
<img width="1783" height="780" alt="image" src="https://github.com/user-attachments/assets/11652fec-e616-4fda-a12a-7e495cd3cdb0" />

✅ GTKWave installed successfully

---

### 4️⃣ Ngspice – Circuit Simulator
**Purpose:** Performs analog and mixed-signal circuit simulations.

**Installation:**  
```bash
$ sudo apt update  
$ sudo apt install ngspice  
```
**Verification:**  
```bash
$ ngspice  
```
<img width="925" height="282" alt="image" src="https://github.com/user-attachments/assets/326d4ae6-01ea-4689-aa74-a28fbed8b5dc" />

✅ Ngspice installed successfully


---


### 5️⃣ Magic VLSI – Layout Tool
**Purpose:** Creates, edits, and analyzes VLSI layouts with DRC capabilities.

**Installation:**  
```bash
$ sudo apt-get install m4 tcsh csh libx11-dev tcl-dev tk-dev \
    libcairo2-dev mesa-common-dev libglu1-mesa-dev libncurses-dev  
$ git clone https://github.com/RTimothyEdwards/magic  
$ cd magic  
$ ./configure  
$ make  
$ sudo make install  
```
**Verification:**  
```bash
$ magic -d XR  
```
<img width="1710" height="913" alt="image" src="https://github.com/user-attachments/assets/19f4265f-715f-4066-b512-203dcd9ec48b" />

✅ Magic VLSI installed successfully


---


### 🎉 Installation Summary

| Tool | Status | Primary Use |
|------|--------|-------------|
| Yosys | ✅ Complete | RTL Synthesis |
| Icarus Verilog | ✅ Complete | Verilog Simulation |
| GTKWave | ✅ Complete | Waveform Analysis |
| Ngspice | ✅ Complete | Circuit Simulation |
| Magic VLSI | ✅ Complete | Layout Design & DRC |
