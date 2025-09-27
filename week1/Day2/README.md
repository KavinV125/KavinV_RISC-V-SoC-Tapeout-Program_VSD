Here’s your rephrased content in a clean **GitHub README.md** format with proper structuring, simplified sentences, and developer-focused clarity:  

***

# Digital Synthesis with Yosys

This guide covers the basics of digital synthesis using **Yosys** with the SkyWater SKY130 standard cell library. It explains the library format, PVT corners, synthesis styles, submodule-level flow, and handling flip-flops with asynchronous reset.

***

## Table of Contents
1. [Library Overview](#library-overview)  
2. [PVT Corners](#pvt-corners)  
3. [Synthesis Styles](#synthesis-styles)  
   - [Hierarchical Synthesis](#hierarchical-synthesis)  
   - [Flat Synthesis](#flat-synthesis)  
4. [Submodule Synthesis](#submodule-synthesis)  
5. [DFF with Asynchronous Reset](#dff-with-asynchronous-reset)  

***

## Library Overview

The synthesis uses the standard cell library:

```
sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Naming Breakdown
- **sky130_fd_sc_hd** → SkyWater 130nm process, standard cells, high-density variant  
- **tt** → Typical process corner  
- **025C** → Characterized at 25°C  
- **1v80** → Characterized at 1.80 V  

The `.lib` file provides:  
- Logic functions (AND, OR, DFF, etc.)  
- Timing data (propagation, setup, hold)  
- Power models  
- Cell area  

During synthesis, Yosys maps RTL into these real hardware cells to generate a **gate-level netlist**.

***

## PVT Corners

**PVT = Process, Voltage, Temperature**  

These parameters capture how real-world variations affect timing, power, and reliability.  

| Corner | Process | Voltage | Temp | Usage |
|--------|---------|---------|------|-------|
| **FF** | Fast    | 1.98 V  | -40°C | Best-case delay (fastest) |
| **TT** | Typical | 1.80 V  | 25°C  | Nominal operation |
| **SS** | Slow    | 1.62 V  | 125°C | Worst-case delay (slowest) |

Designs must pass checks across multiple PVT corners.

***

## Synthesis Styles

Yosys supports two main approaches:

### Hierarchical Synthesis

- Keeps module boundaries intact  
- Easy debugging and modular design  
- Optimizes within each module  

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v 
synth -top multiple_modules
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show multiple_modules
write_verilog multiple_modules_hier.v
```
### Flat Synthesis

- Flattens all hierarchy into one level  
- Enables global optimization  
- Netlist is less readable but more efficient for final implementation  

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
flatten
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog multiple_modules_flat.v
```


***

## Submodule Synthesis

Instead of synthesizing the entire top module, you can synthesize submodules individually.  

**Benefits:**
- Large designs can be broken down for faster synthesis  
- Reusable netlists when a submodule is instantiated multiple times  
- Easier debugging and modular verification  

**Example for `sub_module1`:**

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show sub_module1
write_verilog sub_module1_netlist.v
```

***

## DFF with Asynchronous Reset

### RTL Definition

```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);
always @ (posedge clk, posedge async_reset) begin
    if (async_reset)
        q <= 1'b0;
    else    
        q <= d;
end
endmodule
```

- `clk`: Captures input on rising edge  
- `async_reset`: Immediately resets output to `0` when active  
- `q`: Flip-flop output  

Reset has higher priority over clock input.

***

### Synthesis Flow

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog dff_asyncres_net.v
```

### Why `dfflibmap`?

- Maps generic Yosys flip-flops to real Sky130 cells  
- Ensures correct timing, power, and area usage  
- Produces a **technology-specific netlist** suitable for place & route  

***

### Simulation

Compile and simulate to verify asynchronous reset:

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

Expected behavior:  
- `q` follows `d` on rising clock edges  
- `q` resets immediately when `async_reset` is asserted  


The same synthesis flow applies for:  

| Variant        | Behavior |
|----------------|----------|
| `dff_syncres`  | DFF with synchronous reset |
| `dff_asyncset` | DFF with asynchronous set  |
| `dff_syncset`  | DFF with synchronous set   |

Only the RTL changes; the synthesis steps remain identical.  

***

Would you like me to also format this README with **badges (toolchain versions, PDK info, etc.)** at the top like some open-source VLSI repositories use?
