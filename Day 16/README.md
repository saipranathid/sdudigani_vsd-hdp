
<details>
  <Summary><strong> Day 16 : Good Floorplan vs Bad Floorplan and Introduction to Library cells</strong></summary>

# Contents
- [Chip Floor planning considerations](#chip-floor-planning-considerations)
  - [Utilization factor and aspect ratio](#utilization-factor-and-aspect-ratio)
  - [Pre-placed cells](#cencept-of-pre--placed-cells)
  - [De-coupling Capacitors](#de--coupling-capacitors)
  - [Power planning](#power-planning)
  - [Pin-placement and logical cell placement blockage](#pin--placement-and-logical-cell-placement-blockage)
- [Floorplanning using OpenLANE for picorv32a](#floorplan-using-openlane-for-picorv32a)

<a id="chip-floor-planning-considerations"></a>
# Chip Floor planning considerations

<a id="utilization-factor-and-aspect-ratio"></a>
## Utilization factor and aspect ratio
**Define width and height of core and die:**
- Let us consider a minimal design consisting of two flip-flops (FF) feeding two standard-cell gates (A1, O1). The netlist defines connectivity but says nothing about physical dimensions.
![Alt Text](images/1_eg_netlist.png)

- Enclose each logical element in a rectangular “footprint.” For rough estimation, we assume every cell (FF or gate) is a 1 unit × 1 unit square.
![Alt Text](images/2_phy_dimensions.png)

By convention:
- Standard cell = 1 unit × 1 unit → 1 unit²
- Flip-flop = 1 unit × 1 unit → 1 unit²
![Alt Text](images/3_std_cell_area.png)

- Tile the four 1 unit² blocks into a 2×2 array.
  - Core width = 2 units
  - Core height = 2 units
  - Core area = 4 unit²
![Alt Text](images/4_rough_min_area_cal_of_netlist.png)

The above figure shows the rough calculation of minimum area that is occupied bu the netlist.

- **Core:** the region containing all standard cells (our 2×2 tile).
- **Die:** the die includes the core plus I/O pads, power rings, and metal guard-bands.
- **Wafer:** multiple dice are fabricated together on a circular wafer.
![Alt Text](images/5_core_die_in_chip.png)

![Alt Text](images/6_uti_formula.png)

- In this example, the four blocks completely occupy the core area (4 unit² occupied / 4 unit² total = 1.0 → 100 %).
![Alt Text](images/7_uti_cal.png)

**Note:** Real designs typically target 60–80 % utilization to leave room for routing nets, filler cells, and power straps etc.

- **Aspect ratio** wiil decide the size and shape of the chip. It is the ratio of vertical routing resources to the horizontal routing resources. If its value is 1 then the chip is in square shape and if it is greater than 1 then the chip is in rectangular shape.

$$  
\text{Aspect Ratio}
\=\
\frac{\text{Height of the core area}}
     {\text{Width of the core area}}
$$

- **Core Utilization** defines the area occupied by macros, standard cells and other cells. If Core utilisation is 70% - 70% of core area is used for placing the standard cells, macros and other cells while remaining 30% can be used for routing. In other words it is the area occupied by the netlist.

$$  
\text{Utilization Factor}
\=\
\frac{\text{Area Occupied by Netlist}}
     {\text{Total Core Area}}
$$

![Alt Text](images/8_uti25_aspectration1.png)

<a id="cencept-of-pre--placed-cells"></a>
## Pre-placed cells
- Before running automated placement & routing (APR), we often “pre-place” large or critical blocks (IPs) at fixed locations.
- **Pre-placed cells** are large timing-critical blocks (like memories, clock-gating cells, or custom macros) that are fixed at specific locations in design floorplan before running automated placement and routing. By “black-boxing” each block - exposing only its I/O pins and hiding its internal gates they are ensured that APR treats it as a fixed macro, giving us predictable timing, power-grid alignment, and routing channels around those anchored blocks.

![Alt Text](images/preplaced_cells_1.png)
![Alt Text](images/preplaced_cells_2.png)
![Alt Text](images/preplaced_cells_3.png)
![Alt Text](images/preplaced_cells_4.png)

- Functionality of pre-placed cells is implemented only once and APR tools do not alter their locations.
- The location of pre-placed cells are defined depending upon the design scenario or background.

![Alt Text](images/preplaced_cells_5.png)

<a id="de--coupling-capacitors"></a>
## De-coupling Capacitors

- Decouples the circuit from the V<sub>DD</sub> rail.
- Reduce Zpdn for the required frequencies of operation
- Serve as a charge reservoir for the switching current demands that the VDD rail cannot satisfy.
- Surround pre-placed cells with Decaps to compensate for the switching current demands (di/dt)

![Alt Text](images/decap_1.png)

<a id="power-planning"></a>
## Power planning

 - SSN
   - L*di/dt
     * Discharging : Ground bounce
     * Charging    : Voltage Droop
   - **Solution:** Reduce the Vdd/ Vss parasitics ->
     * Power grid
     * Multiple VDD, VSS pins/ balls 

![Alt Text](images/powerplan_3.png)

![Alt Text](images/powerplan_4.png)

![Alt Text](images/powerplan_5.png)


<a id="pin--placement-and-logical-cell-placement-blockage"></a>
## Pin-placement and logical cell placement blockage

![Alt Text](images/pin_plc_1.png)
![Alt Text](images/pin_plc_2.png)
![Alt Text](images/pin_plc_3.png)
![Alt Text](images/pin_plc_4.png)
![Alt Text](images/pin_plc_5.png)

<a id="die-area-in-microns"></a>
**Calculate the die area in microns from the values in floorplan def**

![Alt Text](images/7_spm_def.png)

- Die Area from spm.def 
    unit to micron scale: 1000 units = 1 micron
    Die area: (0 0) to (101850 112570)

- Calculated dimensions:
    Die width = 101850/1000 = 101.85 μm
    Die height = 112570/1000 = 112.57 μm
    Die Area = 101.85 μm × 112.57 μm = 11465.2545 µm²

<a id="view-test-design-outputs"></a>
**Viewing Test Design Outputs**
- Open the spm.gds using KLayout

```bash
klayout /home/sdudigani/openlane_build_script/work/tools/openlane_working_dir/OpenLane/designs/spm/runs/RUN_2025.07.12_21.05.18/results/final/gds/spm.gds
```
![Alt Text](images/klayout_1.png)


<a id="floorplan-using-openlane-for-picorv32a"></a>
# Floorplanning using OpenLANE for `picorv32a`

