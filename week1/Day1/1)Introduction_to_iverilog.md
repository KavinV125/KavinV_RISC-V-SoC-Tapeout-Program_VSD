## **Day 1: Introduction to Icarus Verilog (iverilog)**

**Goal:** Learn how Icarus Verilog is used for RTL simulation and understand the role of testbenches.

---

### **Key Topics**

#### **1. Simulator**

- A **simulator** is a tool that helps in **validating and verifying a digital design**.
    
- In this context, we use **Icarus Verilog (`iverilog`)** as the simulator.
    

---

#### **2. Testbench**

- A **testbench** checks whether the design meets its specifications.
    
- It applies stimulus (inputs) to the design and observes the outputs.
    
- Unlike the actual design:
    
    - The **design** can have primary inputs and outputs.
        
    - The **testbench** has **no primary inputs or outputs**.

![Week_1/Day_1/Pictures/Testbench.png](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/44eb8eba60827847d89900a6daf173d6fc72013b/week1/Day1/pictures/Testbench.png)

---

#### **3. Simulation Process**

- The simulator **tracks changes in the input signals**.
    
- Whenever an input changes, the **corresponding output is recalculated**.
    
- If the inputs remain unchanged, the output also stays constant.

![image](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/eabdb9ca30b114b73de77a52036b914893e414a7/week1/Day1/pictures/Iverilog.png)    

---

### **Important Notes**

- A **design module** can contain one or more primary inputs and outputs.
    
- A **testbench** does **not** have primary inputs or outputs.
    

---

#### **Basic Simulation Flow**

```bash

# Step 1: Compile the Verilog design along with its testbench
  iverilog -o output_file design.v testbench.v

# Step 2: Run the simulation and generate a VCD file
  vvp output_file

# Step 3: Open the VCD file in GTKWave for waveform analysis
  gtkwave output_file.vcd

```

👉 The **VCD file (`.vcd`)** stores signal transitions over time, which can be visualized in **GTKWave**. This allows you to verify the behavior of your RTL design before proceeding to synthesis.
