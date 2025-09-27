
# Logic Optimization with Yosys

This README explains how to use **`opt_clean -purge`** in Yosys to optimize logic designs.  
It demonstrates three categories of optimizations:

1. Combinational Logic Optimization  
2. Sequential Logic Optimization  
3. Unused Logic Optimization  

---

## Table of Contents

* [Combinational Logic Optimization](#combinational-logic-optimization)  
* [Sequential Logic Optimization](#sequential-logic-optimization)  
* [Unused Logic Optimization](#unused-logic-optimization)  
* [Optimization Flow](#optimization-flow)  
* [Summary Table](#summary-table)  

---

## Combinational Logic Optimization

**Example File:** `opt_check.v`

```
module opt_check (input a , input b , output y);
    assign y = a ? b : 0;
endmodule
```

**Goal:**  
Simplify redundant combinational logic expressions.

**Yosys Flow:**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog opt_check_before_purge.v   # Before purge optimization
opt_clean -purge                         # Remove redundant/unused logic
write_verilog opt_check_after_purge.v    # After purge optimization
show
```

**What `opt_clean -purge` does:**  
- Eliminates unused logic and dead connections  
- Removes redundant gates  
- Reduces area and improves efficiency  

---

## Sequential Logic Optimization

**Example File:** `dff_const1.v`

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
    if (reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end
endmodule
```

**Goal:**  
Simplify flip-flop behavior with constant outputs.

**Yosys Flow:**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog dff_const1_before_purge.v
opt_clean -purge
write_verilog dff_const1_after_purge.v
show
```

---

## Unused Logic Optimization

**Example File:** `counter_opt.v`

```
module counter_opt (input clk, input reset, output q);
reg [2:0] count;
assign q = count;

always @(posedge clk, posedge reset)
begin
    if (reset)
        count <= 3'b000;
    else
        count <= count + 1;
end
endmodule
```

**Goal:**  
Remove counter bits and registers that do not contribute to outputs.

**Yosys Flow:**

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog counter_opt_before_purge.v
opt_clean -purge
write_verilog counter_opt_after_purge.v
show
```
---

## Optimization Flow

```
RTL Code → Synthesis → Netlist → opt_clean -purge → Optimized Netlist
```

---

## Summary Table

| Optimization Type | Example Module | What Gets Optimized                |
| ----------------- | -------------- | ---------------------------------- |
| Combinational     | `opt_check`    | Removes redundant logic gates       |
| Sequential        | `dff_const1`   | Simplifies constant flip-flop paths |
| Unused Logic      | `counter_opt`  | Eliminates unused registers/wires   |

---

## Conclusion

- **`opt_clean -purge`** is a key step in Yosys optimization.  
- It reduces design size by removing unused or redundant logic.  
- Helps generate efficient, cleaner netlists before technology mapping or final P&R steps.  

---
