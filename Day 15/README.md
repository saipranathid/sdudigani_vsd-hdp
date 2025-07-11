<details>
  <Summary><strong> Day 15 : Inception of open-source EDA - OpenLANE and Sky130PDK</strong></summary>

# Contents
- [How to Talk to Computers](#how-to-talk-to-computers)
  - [Introduction to QFN-48 Package - Chip - Pads - Core - Die and IPs](#introduction-to-qfn--48-package--chip--pads--core--die-and-ips)
  - [Introduction to RISC-V](#introduction-to-risc--v)
    - [ISA (instruction Set Architecture)](#isa)
  - [From Software Applications to Hardware](#from-software-applications-to-hardware)   
- [SoC Design and OpenLANE](#soc-design-and-openlane)
  - [Introduction to all Components of open-source digital ASIC design](#introduction-to-all-components-of-open--source-digital-asic-design)
  - [Simplified RTL2GDS flow](#simplified-rtl2gds-flow)
  - [OpenLANE ASIC design flow](#openlane-detailed-asic-design-flow)
- [Get Familiar to open-source EDA tools](#get-familiar-to-opensource-eda-tools)
  - [OpenLANE Directory structure in detail](#openlane-directory-structure-in-detail)
  - [Design Preparation Step](#-design-preparation-step)
  - [Review files after design prep and run synthesis](#review-files-after-design-prep-and-run-synthesis)
  - [OpenLANE Project Git Link Description](#openlane-project-git-link-description)
  - [Steps to Characterize synthesis results](#steps-to-characterize-synthesis-results)

<a id="how-to-talk-to-computers"></a>
# How to Talk to Computers

<a id="introduction-to-qfn--48-package--chip--pads--core--die-and-ips"></a>
## Introduction to QFN-48 Package - Chip - Pads - Core - Die and IPs

**Package:** In any embedded board we have seen, the part of the board we consider as the chip is only the PACKAGE of the chip which is nothing but a protective layer or packet bound over the actual chip and the actual manufatured chip is usually present at the center of a package wherein, the connections from package is fed to the chip by WIRE BOUND method which is none other than basic wired connection.

![Alt Text](images/1.png)

The architecture inside the arduino chip is shown below

![Alt Text](images/2.png)

### QFN-48 (Quad Flat No-Leads) Package
The QFN-48 is a compact, high-performance IC package offering 48 solder-able pads on a 7 mm × 7 mm footprint. Its leadless “no-leads” design minimizes PCB real estate while providing excellent thermal and electrical characteristics.

The architecture inside the processor/ Soc is shown below. Various packages are available and the chip is present inside the package as shown in the diagram below.

![Alt Text](images/3_package_example.png)

**Key Features:**
- Leadless Design: Ultra-low profile; ideal for space-constrained PCBs
- 48 Connection Pads: Rich I/O for complex systems
- Compact Size: 7 mm × 7 mm footprint
- Thermal Efficiency: Exposed pad and copper slug optimize heat dissipation
- Electrical Performance: Low parasitic inductance and resistance

**Common Applications:**
- Microcontroller and microprocessor modules
- Wireless RF front-ends
- Power-management ICs
- High-density sensor interfaces
- Precision data-converter packages

**Chip Overview**
Beneath the package sits the bare silicon die - a landscape of transistors and interconnects implementing everything from logic to memory to analog front-ends. This single piece of silicon handles computation, storage, and I/O.

**Core Functional Blocks:**
- Processing Units: One or more CPU cores (e.g., RISC-V, ARM) execute instructions and control data flow.
- Memory: SRAM, ROM, or flash cells store code, data, and configuration.
- I/O Interfaces: Digital GPIOs, high-speed serial links (QSPI, UART), and analog converters connect to the outside world.

The boundaries of the chip is connected to the pins present in the boundaries of the package.
![Alt Text](images/4.png)
![Alt Text](images/5.png)

**Chip Components Overview**
1. **Pads:** Small metal lands on the package periphery. Serve as the electrical interface between PCB traces and on-die interconnect
2. **Core:** Central silicon region containing CPU, bus fabric, and on-chip peripherals. Floorplanned for optimal timing, power, and area
3. **Die:** The complete silicon piece before packaging. Contains all active circuits, passive components, and metal routing layers

  
![Alt Text](images/6_risc-v_soc.png)

**Foundry IPs:** Pre-characterized circuit blocks supplied by the foundry. Delivered as GDSII, LEF/DEF and timing libraries, these IPs accelerate design by providing plug-and-play analog and mixed-signal functionality.

**Macros:** Macros are large functional blocks designed by the SoC team (or third-party vendors) to meet specific on-chip requirements - such as custom SRAM banks, DMA controllers, or specialized accelerators.

### Macros vs. Foundry IPs Comparison

Below is a side-by-side comparison of in-house **macros** and **foundry IPs**, formatted as a GitHub-friendly Markdown table.

| **Feature**           | **Macros**                                                                                                                                  | **Foundry IPs**                                                                                                                           |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| **Definition**        | Pre-implemented functional blocks (e.g., custom SRAM banks, DMA controllers, accelerators) integrated at the subsystem level                  | Pre-characterized, silicon-proven blocks (e.g., ADC, PLL, high-speed PHYs) provided by the foundry                                           |
| **Source**            | Designed in-house or by third-party IP vendors                                                                                               | Developed, validated, and licensed directly by semiconductor foundries (e.g., TSMC, GlobalFoundries)                                        |
| **Complexity**        | Medium to high (e.g., custom logic, large memories, specialized accelerators)                                                                 | Can range from basic I/O cells to complex analog/digital subsystems (e.g., USB PHY, DDR PHY, PLL)                                           |
| **Customization**     | Highly configurable—parameters and micro-architecture can be tuned for specific power, performance, or area (PPA) goals                        | Limited parameterization—typically voltage range, bit-width, or process corner settings                                                      |
| **Integration Scope** | Integrated and verified at the SoC-level context (requires SoC-wide DRC/LVS, STA, and co-simulation with surrounding logic)                 | Delivered as “black-box” models (GDSII, LEF/DEF, Liberty) ready for drop-in use, requiring minimal SoC-level integration effort             |
| **Verification**      | Must be validated within the SoC—DRC/LVS, STA, power analysis, and functional verification in target use-cases                                | Pre-verified by the foundry across multiple PVT corners, including DRC, LVS, timing, and reliability tests                                   |
| **Purpose**           | Tailored to unique design requirements—e.g., low-power accelerators, custom memories, on-chip bus controllers                                 | Accelerate time-to-market by reusing proven, reusable building blocks, reducing design risk and development time                             |



<a id="introduction-to-risc--v"></a>
## Introduction to RISC-V
<a id="isa"></a>
### ISA (instruction Set Architecture)
The ISA is the “language” of the computer - the interface through which software talks to hardware. When you write C code, it must be executed on a specific processor layout. First, the compiler translates your C into RISC-V assembly; next, an assembler converts that into binary machine code, which is then fed to the processor to produce the required output.

Between the abstract RISC-V specification and the physical layout, we use a hardware description language (HDL) such as Verilog or VHDL. In this flow, the RTL description implements the RISC-V ISA, and that RTL is then synthesized and placed-and-routed to generate the final silicon layout.
![Alt Text](images/isa.png)

<a id="from-software-applications-to-hardware"></a>
## From Software Applications to Hardware
To run a software application on real silicon, high-level code must be transformed—step by step—into transistor-switching signals.  In modern systems this chain looks like:
1. **Application Software**  
   Written in C, C++, Java, etc., and used to implement user-facing functionality (e.g., a web browser or stopwatch).

2. **System Software**  
   Acts as the bridge between your app and the bare metal:
   - **Operating System (OS)**  
     Manages I/O, memory allocation, system calls, and resource scheduling.  
   - **Compiler**  
     Translates your high-level source into target-specific assembly (e.g., RISC-V instructions).  
   - **Assembler**  
     Converts that assembly into binary machine code, ready for the processor.  

3. **Instruction Set Architecture (ISA)**  
   The ISA (here, **RISC-V**) defines the exact binary opcodes your CPU core understands—this is the “language” in which your compiled code speaks to the hardware.

4. **Hardware Description & RTL**  
   A Hardware Description Language (HDL) like Verilog implements the ISA at the register-transfer level (RTL), describing how each instruction maps to flip-flops, adders, and control logic.

5. **Physical Design**  
   RTL is synthesized into a gate-level netlist, then placed, routed, and finally taped out in silicon.

![Alt Text](images/sys_sw.png)

**Example: Stopwatch App on RISC-V**
For example, consider a **stopwatch app** running on a **RISC-V core**. The user writes a simple function in C to implement timekeeping logic (hours, minutes, seconds). This high-level application code is first handled by the **system software**, including:

- **Operating System (OS)**:  
  Manages low-level operations like memory allocation, I/O handling, and system calls (e.g., `sleep()` and `clear()` in the C code).

- **Compiler**:  
  Translates the high-level C code into **RISC-V-specific assembly instructions** tailored to the target architecture.

- **Assembler**:  
  Converts the human-readable assembly code into **binary machine instructions**.

- **Linker**:  
  Combines all object files and dependencies into the final **`.exe` or binary executable**.

This **machine-level binary** is then fed to the **hardware layer**, where it is executed by the RISC-V processor. In physical design workflows, these binary instructions are synthesized and mapped into a **chip layout** using tools like:

- **OpenLane** – For RTL-to-GDSII flow
- **Sky130 PDK** – A 130nm open-source process design kit

Finally, the generated **layout is fabricated into silicon**, producing a chip that can independently execute the stopwatch functionality at the hardware level.

This demonstrates the full-stack hardware design flow:  
**from software → to compiler → to silicon.**

![Alt Text](images/stop_watch.png)

For the above stopwatch the below figure shows the input and output of the compiler and assembler.

![Alt Text](images/stop_watch_2.png)

This image demonstrates the complete transformation of a machine instruction (e.g., add x6, x10, x6) into real, executable hardware logic. At the top, the instruction is part of a RISC-V program defined by the Instruction Set Architecture (ISA) — the abstract interface between software and hardware. The assembler converts these instructions into binary machine code (e.g., 010001101...), which is then interpreted by the RTL (Register Transfer Level) hardware description written in Verilog. This RTL is synthesized into a gate-level netlist, comprising logic gates like NAND, NOR, and flip-flops. Finally, the logic is placed and routed into a physical layout on silicon — shown at the bottom right — where real transistors switch to implement the behavior defined by the instruction. This showcases how a single line of code flows from abstract software into concrete hardware functionality.

![Alt Text](images/stop_watch_3.png)


<a id="soc-design-and-openlane"></a>
# SoC Design and OpenLANE

<a id="introduction-to-all-components-of-open--source-digital-asic-design"></a>
## Introduction to all Components of open-source digital ASIC design
![Alt Text](images/open_source_digital_asic_design_1.png)
In a state-of-the-art digital ASIC design methodology, three categories of inputs converge within EDA toolchains to yield a manufacturable layout and GDSII database:
- RTL IP's
- EDA Tools
- PDK Data

**What is PDK?**
- Process Design Kit (PDK) is the collection of files used to model a fabrication process for the EDA tools used to design an IC. Typical PDK components include:
  - Process design rules: DRC, LVS, PEX
  - Device Models : SPICE models for transistors, diodes, capacitors, resistors, etc.  
  - Digital Standard Cell Libraries: Liberty (.lib) timing models, LEF abstract views, GDSII layouts for each cell  
  - I/O libraries: Specialized cells for pads, ESD protection, level shifters, SERDES PHYs, etc.
- PDK serves as the interface between the FAB and the designers.

![Alt Text](images/open_source_digital_asic_design_2.png)

<a id="simplified-rtl2gds-flow"></a>
## Simplified RTL2GDS flow

This diagram illustrates the core steps in a typical RTL-to-GDSII ASIC implementation flow, using your RTL source and the foundry’s PDK as primary inputs:

![Alt Text](images/open_source_digital_asic_design_3.png)

1. **RTL Synthesis:** Map RTL (Verilog/VHDL) into a gate-level netlist using the standard-cell library from the PDK. Perform technology mapping, logic optimization, and area/timing trade-offs.

![Alt Text](images/synthesis.png)
![Alt Text](images/synthesis_1.png)
3. **Floor & Power Planning:** Partitions the chip area, places key components (macros/IPs), and defines the power grid and I/O pad placement. This step aims to reduce power consumption and improve signal integrity by optimizing physical layout.

![Alt Text](images/chip_fp.png)
![Alt Text](images/fp.png)
![Alt Text](images/pp.png)

4. **Placement:** Assigns physical locations to standard cells, targeting minimal wirelength, low signal delay, and better area utilization. A well-placed design improves performance, reduces congestion, and eases routing complexity.

![Alt Text](images/plc.png)

Global placement provide approximate locations for all cells based on connectivity but in this stage the cells may be overlapped on each other and in detailed placement the positions obtained from global placements are minimally altered to make it legal (non-overlapping and in site-rows)

![Alt Text](images/plc_1.png)

6. **Clock Tree Synthesis (CTS):** Builds a clock distribution network to deliver the clock signal uniformly to all sequential elements like flip-flops and registers. CTS ensures minimal skew, balanced paths, and robust clock propagation.

![Alt Text](images/cts_1.png)

8. **Routing:** Connects all placed components based on netlist connectivity. The router optimizes wire paths for signal integrity, avoids congestion, and satisfies design rule constraints set by the foundry.

![Alt Text](images/routing.png)

skywater PDK has 6 routing layers in which the lowest layer is called the local interconnect layer which is a Titanium Nitride layer the following 5 layers are all Aluminium layers.

![Alt Text](images/routing_1.png)

9. **Sign-off:** Final validation stage - Timing analysis, Power analysis and Physical verification

![Alt Text](images/signoff.png)

11. **GDSII File Generation:** Produces the GDSII file containing all physical layout data. This file is used by foundries to generate photomasks and manufacture the silicon chip. The GDSII is essentially the final blueprint for chip fabrication.

<a id="openlane-detailed-asic-design-flow"></a>
## OpenLANE ASIC design flow
![Alt Text](images/open_lane_1.png)

OpenLANE flow is an automated RTL2GDSII flow where all required tools are embedded into it and you have complete control of each process. We control them by using env variables which will be discussed at each stage since they are unique for each of them. This OpenLANE flow is specially designed for no human interaction based RTL2GDSII flow. Hence we have automated mode and interactive mode to run the OpenLANE flow. 

- Designing an ASIC is a complex and fascinating process that entails various steps, from idea to the fabrication data.
- This process is filled with engineering challenges that require expertise and attention to detail. The entire process requires significant expertise and experience in chip design and can take several months to complete.
- The ASIC design flow is crucial to ensure successful ASIC design. It is based on a comprehensive understanding of ASIC specifications, requirements, low-power design, and performance.
- Engineers can streamline the process and meet crucial time-to-market goals by following a proven ASIC design flow.
- Each stage of the ASIC design cycle is supported by powerful EDA (Electronic Design Automation) tools that facilitate the implementation of the design. The following are examples of steps needed to realize an ASIC.

- **Design Entry**: In this step, the logic design is described using a Hardware Description Language (HDL) like System Verilog. Typically, the description is done at the data flow (Register Transfer) or behavioral levels.

- **Functional Verification**: It is essential to catch design errors early on. The description must be checked against the requirements, which can be done through simulation or formal methods. Functional verification is performed on the RTL description as well as the netlists generated by the following steps.

- **Synthesis:** In this step, the HDL description is converted into a circuit of the logic cells called the Netlist.

- **Layout/Physical Synthesis:** Also called Physical Implementation. In this step, the logic circuit is converted into a layout of the photo masks used for fabrication. This complex step involves several sub-steps typically automated using its flow. These steps include Floorplanning, Placement, Clock-tree synthesis and Routing. Because Placement and Routing are the most time-consuming operations, sometimes we refer to this step as “Placement and Routing”, or PnR.

- **Signoff:** marks the final stage in the rigorous journey of an ASIC’s design; it ensures your creation functions flawlessly, operates efficiently, and ultimately delivers on its promise before sending your chip blueprint off to be carved in silicon.

![Alt Text](images/OL.png)


![Alt Text](images/OL_pnr.png)
![Alt Text](images/OL_antenna_rule_violations.png)
![Alt Text](images/OL_antenna_rule_violations_1.png)
![Alt Text](images/OL_antenna_rule_violations_2.png)
![Alt Text](images/OL_signoff_1.png)
![Alt Text](images/OL_signoff_2.png)

<a id="get-familiar-to-opensource-eda-tools"></a>
# Get Familiar to open-source EDA tools

<a id="openlane-directory-structure-in-detail"></a>
## OpenLANE Directory structure in detail

<a id="-design-preparation-step"></a>
## Design Preparation Step

<a id="review-files-after-design-prep-and-run-synthesis"></a>
## Review files after design prep and run synthesis

<a id="openlane-project-git-link-description"></a>
## OpenLANE Project Git Link Description

<a id="steps-to-characterize-synthesis-results"></a>
## Steps to Characterize synthesis results
