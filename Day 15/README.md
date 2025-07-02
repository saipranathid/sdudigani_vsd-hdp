<details>
  <Summary><strong> Day 15 : Inception of open-source EDA - OpenRoad and Sky130PDK</strong></summary>

# Contents
- [How to Talk to Computers](#how-to-talk-to-computers)
  - [Introduction to QFN-48 Package - Chip - Pads - Core - Die and IPs](#introduction-to-qfn--48-package--chip--pads--core--die-and-ips)
  - [Introduction to RISC-V](#introduction-to-risc--v)
    - [ISA (instruction Set Architecture)](#isa)
  - [From Software Applications to Hardware](#from-software-applications-to-hardware)   
- [SoC Design and OpenRoad](#soc-design-and-openroad)
  - [Introduction to all Components of open-source digital ASIC design](#introduction-to-all-components-of-open--source-digital-asic-design)
  - [Simplified RTL2GDS flow](#simplified-rtl2gds-flow)

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


<a id="soc-design-and-openroad"></a>
# SoC Design and OpenRoad

<a id="introduction-to-all-components-of-open--source-digital-asic-design"></a>
## Introduction to all Components of open-source digital ASIC design

<a id="simplified-rtl2gds-flow"></a>
## Simplified RTL2GDS flow
