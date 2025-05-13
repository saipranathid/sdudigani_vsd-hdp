# Day 1: Introduction to Verilog RTL design and Synthesis

## 1. Introduction to open-source simulator iverilog

### [Introduction to iverilog design test bench]

#### Simulator
- RTL design is checked for adherence to the spec by simulating the design.
- Simulator is the tool used for simulating the design : iverilog is the tool used for this course.

#### Design
- The design refers to the actual Verilog code or a collection of Verilog modules that implement the intended digital functionality.
- This code is written to satisfy the design specifications and meet the required behavior defined in the project or problem statement.

#### TestBench
- A TestBench is a setup used to apply stimulus (also called test vectors) to the design in order to verify its functionality.
- It mimics the input environment and checks whether the design behaves as expected under different conditions.

#### How Simulator Works?
- Simulator looks for the changes on the input signals.
- Upon change to the input, the output is evaluated.
   - If no change to the input, no change to the output!
- Simulator is looking for change in the values of input!

![Alt Text](images/Test_Bench.png)

#### iverilog - Based Simulation Flow
The simulation process involves the following steps:
  1. Design File: Contains the RTL code written in Verilog.
  2. Testbench File: Stimulates the design with input vectors and monitors output.
  3. Both files are compiled using the iverilog tool.
  4. The simulation generates a <strong> .vcd (Value Change Dump)</strong> file that logs all signal transitions over time.
  5. The .vcd file is then visualized using <strong> gtkwave</strong>, a waveform viewer.

![Alt Text](images/iverilog_based_simulation_flow.png)

## 2. Labs Using iverilog and gtkwave

### L1 [Lab 1 - Introduction]
#### Setup
- cd ~
- mkdir VLSI
- cd VLSI
- mkdir vsdflow
- git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

### L2 [Lab 2 - iverilog gtkwave part1]
#### Steps:
1. Navigate to the verilog_files directory
```bash
cd /home/sdudigani/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

2. Compile the Design and Testbench using Icarus Verilog --> This will generate an executable output file named a.out.
```bash
iverilog good_mux.v tb_good_mux.v
```
![Alt Text](images/iverilog_based_simulation_flow.png)

3. Run the Simulation
```bash
./a.out
```
![Alt Text](images/vcd_file_generation.png)

4. View the waveform using gtkwave
```bash
gtkwave tb_good_mux.vcd
```
![Alt Text](images/gtkwave_simulator.png)


### L3 [Lab 2 - iverilog gtkwave part2]

 






