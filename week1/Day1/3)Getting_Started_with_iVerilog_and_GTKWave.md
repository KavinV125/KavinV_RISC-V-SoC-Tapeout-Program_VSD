# **How to Use iVerilog and GTKWave**

To get comfortable with simulation tools, we’ll start by working with the **example Verilog files** included in the cloned repository, located in:

```bash
verilog_files/
```

Each example contains two files:

- **Design file** (e.g., `good_mux.v`) – describes the hardware logic.
    
- **Testbench file** (e.g., `tb_good_mux.v`) – provides stimulus (inputs) to test and verify the design.
    

---

## **Step-by-Step Workflow**

### **Step 1: Move to the Verilog Files Directory**

Open a terminal and navigate to the folder where the Verilog files are stored:

```bash
cd VSD/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

✔ Ensures all commands are run in the correct location.

---

### **Step 2: Select a Design and Testbench**

- Pick any Verilog design file from the folder.
    
- Each design has a corresponding testbench.
    

👉 Example:

- **Design**: `good_mux.v`
    
- **Testbench**: `tb_good_mux.v`
    

---

### **Step 3: Compile Using iVerilog**

Run the following command to compile both the design and its testbench:

```bash
iverilog good_mux.v tb_good_mux.v
```

- **Output:** `a.out` (the compiled simulation executable).
    
- This file contains the simulation program that can now be executed.
    

---

### **Step 4: Run the Simulation**

Execute the simulation:

```bash
./a.out
```

- **Output:** `tb_good_mux.vcd` file.
    
- The **VCD (Value Change Dump)** file records all signal transitions over time.
    

---

### **Step 5: Visualize with GTKWave**

Open the VCD file in GTKWave to view waveforms:

```bash
gtkwave tb_good_mux.vcd
```

- **Output:** A GTKWave window displaying signal waveforms.
    
- Here you can analyze how inputs vary over time and how outputs respond, verifying your design’s correctness.
    

![gtkwave](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/507a0c68fa4764a066495f5788f6069563380bf6/week1/Day1/pictures/WhatsApp%20Image%202025-09-27%20at%209.32.07%20PM(1).jpeg)

---

## **Workflow Diagram**

```bash
Design + Testbench     
    │    
    ▼   
 iVerilog   
    │    
    ▼   
  a.out    
	│    
	▼ 
VCD File 
	│
	▼ 
GTKWave (Waveform)
```

---

## **Step Summary Table**

|Step|Command|Output|Purpose|
|---|---|---|---|
|1|`cd verilog_files`|–|Move into the folder with Verilog files|
|2|Choose design + testbench|–|Select a design and its matching testbench|
|3|`iverilog good_mux.v tb_good_mux.v`|`a.out`|Compile design + testbench|
|4|`./a.out`|`tb_good_mux.vcd`|Run simulation and generate VCD file|
|5|`gtkwave tb_good_mux.vcd`|GTKWave window|View and analyze signal waveforms|

---

## **Exploring Verilog File Structure**

To examine the contents of the Verilog files, open them in a text editor:

```bash
mousepad good_mux.v
```

- Look for **module definitions, input/output ports, and internal logic**.
    

To open the testbench in a separate window:

```bash
mousepad tb_good_mux.v
```
