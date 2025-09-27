
# Gate-Level Simulation (GLS) and Simulation-Synthesis Mismatches

This README explains how to perform **Gate-Level Simulation (GLS)** using Yosys, Icarus Verilog, and GTKWave.  
It also highlights common **simulation-synthesis mismatches** caused by poor RTL coding practices.

---

## Table of Contents

* [GLS: Ternary Operator MUX](#gls-ternary-operator-mux)  
* [Mismatch: Non-Clocked `always`](#mismatch-non-clocked-always)  
* [Mismatch: Blocking Assignment](#mismatch-blocking-assignment)  

---

## Flow: RTL → Synthesis → GLS → Compare Waveforms

```
     RTL Code (Verilog)
             │
             ▼
     RTL Simulation
             │
             ▼
        Synthesis
             │
             ▼
     Gate-Level Netlist
             │
             ▼
 Gate-Level Simulation (GLS)
             │
             ▼
 Compare RTL and GLS Waveforms
```

### Explanation
- **RTL Simulation** checks if the behavioral code is logically correct.  
- **Synthesis** converts RTL into a gate-level netlist mapped to standard cells.  
- **GLS** validates whether the synthesized hardware matches RTL.  
- **Waveform comparison** ensures functional consistency across both stages.  

---

## GLS: Ternary Operator MUX

**Source:** `ternary_operator_mux.v`

```
module ternary_operator_mux (
    input i0, 
    input i1, 
    input sel, 
    output y
);
assign y = sel ? i1 : i0;
endmodule
```

**Purpose:**  
- Clean combinational logic example using a ternary operator.  
- Provides correct RTL vs GLS behavior for demonstration.  

**Synthesis Flow:**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr ternary_operator_mux_net.v
```

**GLS Flow:**

```
iverilog ../my_lib/verilog_model/primitives.v \
        ../my_lib/verilog_model/sky130_fd_sc_hd.v \
        ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

**Note:**  
- `primitives.v`: Provides basic logic gate models.  
- `sky130_fd_sc_hd.v`: RTL models of SkyWater standard cells.  
- `ternary_operator_mux_net.v`: Netlist (output of synthesis).  
- `tb_ternary_operator_mux.v`: Testbench.  

**Results:**  
- RTL and GLS waveforms match.  

---

## Mismatch: Non-Clocked `always`

**Source:** `bad_mux.v`

```
module bad_mux (
    input i0, 
    input i1, 
    input sel, 
    output reg y
);
always @(sel)
begin
    if (sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

**Issue:**  
- Sensitivity list only includes `sel`, but **not** data inputs `i0` and `i1`.  
- RTL simulation works, but synthesis infers unintended hardware, causing mismatches.  

**RTL Simulation:**

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

**Synthesis and GLS:**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr bad_mux_net.v

iverilog ../my_lib/verilog_model/primitives.v \
        ../my_lib/verilog_model/sky130_fd_sc_hd.v \
        bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

**Results:** RTL and GLS waveforms show mismatches.  

---

## Mismatch: Blocking Assignment

**Source:** `blocking_caveat.v`

```
module blocking_caveat (
    input a, 
    input b, 
    input c, 
    output reg d
);
reg x;
always @(*)
begin
    d = x & c;   // evaluated first in simulation
    x = a | b;   // order matters in simulation
end
endmodule
```

**Issue:**  
- Blocking assignments (`=`) run sequentially in simulation.  
- Synthesis tools may reorder or optimize logic, producing **different waveforms**.  

**RTL Simulation:**

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

**Synthesis and GLS:**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr blocking_caveat_net.v

iverilog ../my_lib/verilog_model/primitives.v \
        ../my_lib/verilog_model/sky130_fd_sc_hd.v \
        blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

**Results:**  
Mismatch observed between RTL and GLS.  

---

## Conclusion

- GLS is crucial to check **synthesized hardware behavior** matches RTL.  
- Mismatches usually come from:  
  1. Incomplete sensitivity lists  
  2. Blocking assignments in combinational blocks  
  3. Poor RTL coding practices  

**Best Practices:**  
- Use `always @(*)` for combinational logic.  
- Prefer **non-blocking assignments (`<=`)** in sequential logic.  
- Verify designs with both RTL and GLS to catch mismatches early.  

---
