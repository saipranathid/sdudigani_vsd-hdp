<details>
  <Summary><strong> Day 17 : Cell Design using Magic Layout and ngspice Characterization</strong></summary>

# Contents
- [Cell Design and Characterization Flows](#cell-design-and-char-flow)
  - [Standard Cell Design Flow](#standard-cell-design-flow)
- [Standard Cell Characterization Flow](#sta-cell-char-flow)

<a id="cell-design-and-char-flow"></a>
# Cell Design and Characterization Flows

In an IC design flow, a **library** is a collection of standard cells, each defined by its size, functionality, threshold voltage, and other electrical/physical properties. These libraries are fundamental to the ASIC flow for synthesis, placement, and timing analysis.

![Alt Text](images/std_cells.png)

![Alt Text](images/std_cells_1.png)


**Inputs:**
- PDKs (Process Design Kits)  
  - DRC & LVS rules
  - SPICE Models
- Library & User-Defined specs  
  - eg: cell height, supply voltage, metal layers, pin location, drawn gate-length

<a id="standard-cell-design-flow"></a>
**Standard Cell Design Flow**
1. Circuit Design 
2. Layout Design 
3. Parasitic Extraction
4. Characterization  

**Outputs**
- `CDL` Circuit Description Language (Netlist from circuit design)
- `LEF` Library Exchange Format
- `GDSII` Final Layout Database
- `.cir` Extracted SPICE NEtlist
- Characterized `.lib` files (Timing, Power and Noise) 

![Alt Text](images/std_cell_design_flow_char.png)

<a id="sta-cell-char-flow"></a>
# Standard Cell Characterization Flow
A typical standard cell char process include:
1. Read in SPICE models and tech files
2. Load the extracted SPICE netlist
3. Recognize cell behavior
4. Identify subcircuits
5. Attach power sources
6. Apply stimulus to the setup
7. Set output cap loads
8. Provide necessary simulation commands

![Alt Text](images/char_flow_2.png)

![Alt Text](images/char_flow_1.png)

![Alt Text](images/char_flow_3.png)

All these steps are described in a configuration file and passed to a characterization tool such as GUNA. The tool simulates the cells and generates:
- Timing models
- Power models
- Noise models

These are exported in .lib format and used in synthesis and static timing analysis flows.






</details>
