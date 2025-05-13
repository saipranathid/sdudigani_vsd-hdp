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

### L1 [Lab 1 - Introduction to Lab]

### L2 [Lab 2 - Introduction to iverilog gtkwave part1]

### L3 [Lab 2 - Introduction to iverilog gtkwave part2]

 






