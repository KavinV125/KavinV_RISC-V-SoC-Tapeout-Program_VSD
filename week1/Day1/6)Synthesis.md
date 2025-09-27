# Yosys Synthesis Workflow

---

## Step 1: Complete Command List

```bash
cd verilog_files
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog good_mux_netlist.v
!mousepad good_mux_netlist.v
write_verilog -noattr good_mux_netlist.v
!mousepad good_mux_netlist.v
```

> **Tip:** All commands are listed here first for quick reference.

---

## Step 2: Executing Commands with Outputs

### 1. Move to the Verilog files directory

```bash
cd verilog_files
```

---

### 2. Load the standard cell library

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

### 3. Import RTL design

```bash
read_verilog good_mux.v
```
![image](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/5642d1130f04086cd5be543a2ba18de098e1156f/week1/Day1/pictures/WhatsApp%20Image%202025-09-27%20at%2010.11.46%20PM.jpeg)
---

### 4. Run synthesis on the top module

```bash
synth -top good_mux
```

![image](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/5642d1130f04086cd5be543a2ba18de098e1156f/week1/Day1/pictures/WhatsApp%20Image%202025-09-27%20at%2010.11.45%20PM.jpeg)
![image](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/5642d1130f04086cd5be543a2ba18de098e1156f/week1/Day1/pictures/WhatsApp%20Image%202025-09-27%20at%2010.11.45%20PM(1).jpeg)
---

### 5. Perform technology mapping with ABC

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

### 6. Visualize the synthesized netlist

```bash
show
```

![output](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/5642d1130f04086cd5be543a2ba18de098e1156f/week1/Day1/pictures/WhatsApp%20Image%202025-09-27%20at%2010.11.44%20PM.jpeg)

---

### 7. Save the synthesized netlist (with attributes)

```bash
write_verilog good_mux_netlist.v
```

---

### 8. Open the netlist for review

```bash
!mousepad good_mux_netlist.v
```

![image](https://github.com/KavinV125/KavinV_RISC-V-SoC-Tapeout-Program_VSD/blob/5642d1130f04086cd5be543a2ba18de098e1156f/week1/Day1/pictures/WhatsApp%20Image%202025-09-27%20at%2010.11.44%20PM(1).jpeg)

---

### 9. Export netlist without attributes

```bash
write_verilog -noattr good_mux_netlist.v
```

---

### 10. Open the simplified netlist

```bash
!mousepad good_mux_netlist.v
```

---

## Step 3: Command Breakdown

| Step | Command                                                      | Description                                                |
| ---- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| 1    | `cd verilog_files`                                           | Enter the folder containing the Verilog source files.      |
| 2    | `read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib` | Load the Sky130 standard cell library for synthesis.       |
| 3    | `read_verilog good_mux.v`                                    | Import the RTL code into Yosys.                            |
| 4    | `synth -top good_mux`                                        | Synthesize the design, using `good_mux` as the top module. |
| 5    | `abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib`      | Optimize and map the design to technology-specific gates.  |
| 6    | `show`                                                       | Generate a graphical schematic of the synthesized design.  |
| 7    | `write_verilog good_mux_netlist.v`                           | Save the netlist (with synthesis attributes).              |
| 8    | `!mousepad good_mux_netlist.v`                               | Open the netlist for inspection.                           |
| 9    | `write_verilog -noattr good_mux_netlist.v`                   | Save the netlist without additional attributes.            |
| 10   | `!mousepad good_mux_netlist.v`                               | Review the simplified netlist.                             |

---

## Step 4: Flow Overview

```
RTL Code (good_mux.v)
        │
        ▼
Load Standard Cell Library
        │
        ▼
RTL → Synthesis in Yosys
        │
        ▼
Technology Mapping (ABC)
        │
        ▼
Netlist (good_mux_netlist.v)
        │
        ▼
Verification & Inspection
```

> ✅ The **primary inputs and outputs** are preserved between the RTL and the synthesized netlist, so the same testbench can be reused for verification.
