<details>
  <Summary><strong> Day 3 : Combinational and Sequential optmizations</strong></summary>

## Contents
1. [Introduction to Optimisations](#1-introduction-to-optimisations)
2. [Combinational Logic Optimisation Lab 06](#2-combinational-logic-optimisation-lab-06)
3. [Sequential Logic Optimisation Lab 07](#3-sequential-logic-optimisation-lab-07)
4. [Sequential Optimisations for Unused Outputs](#4-sequential-optimisations-for-unused-outputs)


## 1. Introduction to Optimisations
### Combinational Logic Optimisations
- Combinational logic optimisation focuses on refining logic circuits to achieve a more efficient design in terms of area and power.
#### Key techniques include:
- <strong> Constant Propagation:</strong> Directly simplifying logic paths when input constants are known.
  ```bash
    Example:
    Y = ((A·B) + C)'
    If A = 0 → Y = (0 + C)' = C'
  ```
  
- <strong> Boolean Optimisation:</strong> Using formal methods like:
    - K-Map (Karnaugh Map)
    - Quine-McCluskey Algorithm
  to minimize Boolean expressions and reduce gate count.
  ```bash
    Example: 
    assign y = a ? (b ? c : (c ? a : 0)) : (!c);
    Optimized expression: y = a ⊕ c
  ```

### Sequential Logic Optimisations
- <strong> Basic</strong>
  - Sequential Constant Propogation : Propagates known constant values through flip-flops during synthesis.
  
- <strong> Advanced</strong>
  - State Optimization : Minimizes the number of states in Finite State Machines (FSMs), reducing area and transition overhead.
  - Retiming : Relocates registers across combinational logic boundaries to balance path delays and improve overall timing performance.
  - Sequential Logic Cloning : Duplicates logic paths in floorplan-aware synthesis to meet timing constraints and reduce routing congestion.

## 2. Combinational Logic Optimisation Lab 06
### Optimisation 1
![Alt Text](images/opt_check1_verilog.png)
![Alt Text](images/opt_check1.png)

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v 
synth -top opt_check
opt_clean -purge # to remove unused or redundant logic 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Alt Text](images/opt_check1_synth.png)

### Optimisation 2
![Alt Text](images/opt_check2_verilog.png)
![Alt Text](images/opt_check2.png)

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v 
synth -top opt_check2
opt_clean -purge # to remove unused or redundant logic 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Alt Text](images/opt_check2_synth.png)

### Optimisation 3
![Alt Text](images/opt_check3_verilog.png)
![Alt Text](images/opt_check3.png)

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v 
synth -top opt_check3
opt_clean -purge # to remove unused or redundant logic 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Alt Text](images/opt_check3_synth.png)

### Optimisation 4
![Alt Text](images/opt_check4_verilog.png)
![Alt Text](images/opt_check4.png)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check4.v 
synth -top opt_check4
opt_clean -purge # to remove unused or redundant logic 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Alt Text](images/opt_check4_synth.png)

### Optimisation 5
![Alt Text](images/mm_opt_verilog.png)
![Alt Text](images/mm_opt.png)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check5.v 
synth -top opt_check5
opt_clean -purge # to remove unused or redundant logic 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Alt Text](images/mm_opt_synth.png)

![Alt Text](images/opt_check5_synth.png)

### Optimisation 6
![Alt Text](images/mm_opt2_verilog.png)
![Alt Text](images/mm_opt2.png)


```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check5.v 
synth -top opt_check5
opt_clean -purge # to remove unused or redundant logic 
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

![Alt Text](images/mm_opt2_synth.png)

## 3. Sequential Logic Optimisation Lab 07


## 4. Sequential Optimisations for Unused Outputs
