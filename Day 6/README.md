<details>
  <Summary><strong> Day 6 : VSDBabySoC Post-Synthesis Simulation</strong></summary>

## 📚 Contents
- [Introduction](#introduction)
- [Synthesis using Yosys](#synthesis-using-yosys)
- [Post-Synthesis Simulation](#post-synthesis-simulation)
- [Comparing Pre-Synthesis and Post-Synthesis Output](#comparing-pre-synthesis-and-post-synthesis-output)


## Introduction
Post-synthesis simulation is a critical step in the digital design flow. While pre-synthesis simulation verifies the design logic at the RTL level, post-synthesis simulation ensures that the synthesized gate-level netlist continues to exhibit the correct behavior — now with realistic delays and mapped cells.

### 🔍 Why Pre-Synthesis and Post-Synthesis?
#### Pre-Synthesis Simulation:

- Focuses solely on verifying functionality based on the RTL code.
- Operates in a zero-delay environment; all events occur precisely on clock edges.
- Ideal for checking control/data logic correctness without concern for physical constraints.

#### Post-Synthesis Simulation (GLS - Gate-Level Simulation):

- Uses the synthesized gate-level netlist, incorporating technology-mapped standard cells.
- Captures functional behavior + timing delays, modeling more realistic hardware operation.
- Helps detect:
  - Timing issues like setup/hold violations.
  - Functional mismatches caused by synthesis artifacts (e.g., unintended latches).
  - Dynamic behaviors not visible through static analysis.

For the VSDBabySoC project, synthesis was performed using Yosys to generate the gate-level netlist. This netlist was then simulated using the same testbench as in RTL simulation. The key goal was to compare waveform outputs from both stages and validate that functionality remains consistent. This validation step is essential to ensure that no logic is broken during synthesis and that the design is ready for physical implementation stages like place and route.

## Synthesis using Yosys

The following cp commands copy essential header files from the src/include directory into the working directory. These include:
  - sp_verilog.vh – contains Verilog definitions and macros
  - sandpiper.vh – holds integration-related definitions for SandPiper
  - sandpiper_gen.vh – may include auto-generated or tool-generated parameters

```bash
cd ~/VLSI/VSDBabySoC/
cp -r src/include/sp_verilog.vh .
cp -r src/include/sandpiper.vh .
cp -r src/include/sandpiper_gen.vh .
```

```bash
sdudigani@sdudigani-VirtualBox:~/VLSI/VSDBabySoC$ c
total 100K
-rw-rw-r--  1 sdudigani sdudigani  12K May 24 22:31 LICENSE
-rw-rw-r--  1 sdudigani sdudigani  49K May 24 22:31 README.md
-rw-rw-r--  1 sdudigani sdudigani 6.5K May 24 22:31 Makefile
drwxrwxr-x  2 sdudigani sdudigani 4.0K May 24 22:31 images
drwxrwxr-x 11 sdudigani sdudigani 4.0K May 24 22:31 src
drwxrwxr-x  5 sdudigani sdudigani 4.0K May 24 22:40 sd_env
drwxrwxr-x  4 sdudigani sdudigani 4.0K May 27 11:45 output
-rw-rw-r--  1 sdudigani sdudigani 2.4K May 27 12:02 sp_verilog.vh
-rw-rw-r--  1 sdudigani sdudigani 2.6K May 27 12:02 sandpiper.vh
-rw-rw-r--  1 sdudigani sdudigani  164 May 27 12:03 sandpiper_gen.vh
```

#### ✅ Step 1: Load the Top-Level Design and Supporting Modules
- Launch the Yosys synthesis tool from your working directory.
  ```bash
  cd ~/VLSI/VSDBabySoC/
  yosys
  ```
![Alt Text](images/1_invoke_yosys.png)
  
- Read the main vsdbabysoc.v RTL file into the Yosys environment.
  ```bash
   yosys> read_verilog src/module/vsdbabysoc.v
  ```

![Alt Text](images/2_read_verilog.png)

- Read the rvmyth.v file with the include path using -I option.
  ```bash
  yosys> read_verilog -I ~/VLSI/VSDBabySoC/src/include/ ~/VLSI/VSDBabySoC/src/module/rvmyth.v
  ```
![Alt Text](images/3_read_verilog_rvmyth.png)

- Read the clk_gate.v file with the include path using -I option.
  ```bash
  yosys> read_verilog -I ~/VLSI/VSDBabySoC/src/include/ ~/VLSI/VSDBabySoC/src/module/clk_gate.v
  ```
![Alt Text](images/4_read_verilog_clk_gate.png)

#### ✅ Step 2: Load the Liberty Files for Synthesis
Inside the same Yosys shell, run:
```bash
yosys> read_liberty -lib ~/VLSI/VSDBabySoC/src/lib/avsdpll.lib 
yosys> read_liberty -lib ~/VLSI/VSDBabySoC/src/lib/avsddac.lib 
yosys> read_liberty -lib ~/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

![Alt Text](images/5_read_liberty.png)

#### ✅ Step 3: Run Synthesis Targeting vsdbabysoc
```bash
yosys> synth -top vsdbabysoc
```

![Alt Text](images/6_synth.png)
![Alt Text](images/7_synth.png)
![Alt Text](images/8_synth.png)
![Alt Text](images/9_synth.png)

#### ✅ Step 4: Map D Flip-Flops to Standard Cells
```bash
yosys> dfflibmap -liberty ~/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
![Alt Text](images/10_dff_map.png)

#### ✅ Step 5: Perform Optimization and Technology Mapping
```bash
yosys> opt
yosys> abc -liberty ~/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;{D};strash;dch,-f;map,-M,1,{D}
```
![Alt Text](images/11_opt.png)
![Alt Text](images/12_abc.png)

#### ✅ Step 6: Perform Final Clean-Up and Renaming
```bash
yosys> flatten
yosys> setundef -zero
yosys> clean -purge
yosys> rename -enumerate
```
![Alt Text](images/13_cleanup_renaming.png)

#### ✅ Step 7: Check Statistics

```bash
yosys> stat

13. Printing statistics.

=== vsdbabysoc ===

   Number of wires:               4736
   Number of wire bits:           6210
   Number of public wires:        4736
   Number of public wire bits:    6210
   Number of ports:                  7
   Number of port bits:              7
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:               5920
     $scopeinfo                      8
     avsddac                         1
     avsdpll                         1
     sky130_fd_sc_hd__a2111oi_0     10
     sky130_fd_sc_hd__a211o_2        1
     sky130_fd_sc_hd__a211oi_1      26
     sky130_fd_sc_hd__a21boi_0       4
     sky130_fd_sc_hd__a21o_2         1
     sky130_fd_sc_hd__a21oi_1      672
     sky130_fd_sc_hd__a221o_2        1
     sky130_fd_sc_hd__a221oi_1     163
     sky130_fd_sc_hd__a22o_2         4
     sky130_fd_sc_hd__a22oi_1      123
     sky130_fd_sc_hd__a311oi_1       4
     sky130_fd_sc_hd__a31o_2         1
     sky130_fd_sc_hd__a31oi_1      344
     sky130_fd_sc_hd__a32oi_1        2
     sky130_fd_sc_hd__a41oi_1       26
     sky130_fd_sc_hd__and2_2        12
     sky130_fd_sc_hd__and3_2         1
     sky130_fd_sc_hd__clkinv_1     597
     sky130_fd_sc_hd__dfxtp_1     1144
     sky130_fd_sc_hd__lpflow_inputiso0p_1      1
     sky130_fd_sc_hd__mux2i_1       12
     sky130_fd_sc_hd__nand2_1      839
     sky130_fd_sc_hd__nand3_1      249
     sky130_fd_sc_hd__nand3b_1       1
     sky130_fd_sc_hd__nand4_1       41
     sky130_fd_sc_hd__nor2_1       403
     sky130_fd_sc_hd__nor3_1        35
     sky130_fd_sc_hd__nor4_1         2
     sky130_fd_sc_hd__o2111ai_1     20
     sky130_fd_sc_hd__o211a_1        1
     sky130_fd_sc_hd__o211ai_1      49
     sky130_fd_sc_hd__o21a_1         6
     sky130_fd_sc_hd__o21ai_0      866
     sky130_fd_sc_hd__o21ba_2        1
     sky130_fd_sc_hd__o21bai_1      18
     sky130_fd_sc_hd__o221a_2        1
     sky130_fd_sc_hd__o221ai_1       7
     sky130_fd_sc_hd__o22ai_1      155
     sky130_fd_sc_hd__o2bb2ai_1      1
     sky130_fd_sc_hd__o311ai_0       2
     sky130_fd_sc_hd__o31ai_1        3
     sky130_fd_sc_hd__o32ai_1        1
     sky130_fd_sc_hd__o41ai_1        1
     sky130_fd_sc_hd__or2_2         12
     sky130_fd_sc_hd__or3_2          1
     sky130_fd_sc_hd__or4_2          1
     sky130_fd_sc_hd__xnor2_1       13
     sky130_fd_sc_hd__xor2_1        32
yosys> 
```

#### ✅ Step 8: Write the Synthesized Netlist
```bash
yosys> write_verilog -noattr ~/VLSI/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v
```
![Alt Text](images/14_write_verilog.png)

## Post-Synthesis Simulation
#### ✅ Step 1: Compile the Testbench
- Before running the iverilog command, copy the necessary standard cell and primitive models: These files must be present in the same directory as the testbench (src/module) to resolve all module references during compilation.
  ```bash
  cd ~/VLSI/VSDBabySoC/src/module
  cp -r ~/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v .
  cp -r ~/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/primitives.v .
  ```

- Run the following `iverilog` command to compile the testbench:
  ```bash
  cd ~/VLSI/VSDBabySoC/
  iverilog -o /home/sdudigani/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I /home/sdudigani/VLSI/VSDBabySoC/src/include -I /home/sdudigani/VLSI/VSDBabySoC/src/module /home/sdudigani/VLSI/VSDBabySoC/src/module/testbench.v
  ```
![Alt Text](images/17_compile_tb.png)
##### Note:
You may encounter following errors:

<strong> 🔴 Error 1:</strong>

```bash
sdudigani@sdudigani-VirtualBox:~/VLSI/VSDBabySoC$ iverilog -o /home/sdudigani/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I /home/sdudigani/VLSI/VSDBabySoC/src/include -I /home/sdudigani/VLSI/VSDBabySoC/src/module /home/sdudigani/VLSI/VSDBabySoC/src/module/testbench.v
/home/sdudigani/VLSI/VSDBabySoC/src/module/testbench.v:10: Include file vsdbabysoc.synth.v not found
No top level modules, and no -s option.
```

![Alt Text](images/error_1_include_file_vsdbabusoc_synth_not_found.png)

<strong> 🟢 To resolve this error copy the vsdbabysoc.synth.v from output/post_synth_sim directory to src/module directory</strong> 
  
![Alt Text](images/error_1_sol.png)

<strong> 🔴 Error 2:</strong>

```bash
sdudigani@sdudigani-VirtualBox:~/VLSI/VSDBabySoC$ iverilog -o /home/sdudigani/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out -DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1 -I /home/sdudigani/VLSI/VSDBabySoC/src/include -I /home/sdudigani/VLSI/VSDBabySoC/src/module /home/sdudigani/VLSI/VSDBabySoC/src/module/testbench.v
/home/sdudigani/VLSI/VSDBabySoC/src/module/sky130_fd_sc_hd.v:74452: syntax error
I give up.
```

![Alt Text](images/error_2_i_give_up.png)

<strong> 🟢 To resolve this: Update the syntax in the file sky130_fd_sc_hd.v at or around line 74452.</strong>
<strong> As shown in the images below change:</strong>
```bash
`endif SKY130_FD_SC_HD__LPFLOW_BLEEDER_FUNCTIONAL_V
```

![Alt Text](images/16_sky130_fd_sc_hd_dot_v.png)

<strong> To:</strong>
```bash
`endif //SKY130_FD_SC_HD__LPFLOW_BLEEDER_FUNCTIONAL_V
```

![Alt Text](images/error_2_sol.png)

#### ✅ Step 2: Navigate to the Post-Synthesis Simulation Output Directory
```bash
cd output/post_synth_sim/
```
#### ✅ Step 3: Run the Simulation
```bash
./post_synth_sim.out
```
![Alt Text](images/18_run_simulation.png)
#### ✅ Step 4: View the Waveforms in GTKWave
```bash
gtkwave post_synth_sim.vcd
```
![Alt Text](images/19_post_synth_sim_output.png)

### Comparing Pre-Synthesis and Post-Synthesis Output
#### Pre-Synthesis Simulation Waveform
![Alt Text](images/2.png)

#### Post-Synthesis Simulation Waveform
![Alt Text](images/19_post_synth_sim_output.png)

<strong> Observation:</strong> The outputs of the post-synthesis simulation matched the pre-synthesis simulation waveforms, confirming that the synthesized design maintained the intended functionality of the RTL code.

</details>
