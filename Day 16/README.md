
<details>
  <Summary><strong> Day 16 : Good Floorplan vs Bad Floorplan and Introduction to Library cells</strong></summary>

# Contents
- [Chip Floor planning considerations](#chip-floor-planning-considerations)
  - [Utilization factor and aspect ratio](#utilization-factor-and-aspect-ratio)
  - [Concept of pre-placed cells](#cencept-of-pre--placed-cells)
  - [De-coupling Capacitors](#de--coupling-capacitors)
  - [Power planning](#power-planning)
  - [Pin-placement and logical cell placement blockage](#pin--placement-and-logical-cell-placement-blockage)
  - [Steps-to-run-floorplan-using-OpenLANE](#steps-to-run-floorplan-using-openlane)
  - [Review floorplan files and steps to view floorplan](#review-floorplan-files-and-steps-to-view-floorplan)
  - [Review floorplan layout in magin](#review-floorplan-layout-in-magic)
- [Library Binding and Placement](#library-binding-and-placement)
  - [Netlist binding and initial place design](netlist-binidng-and-initial-place-design)
  - [Optimize placement using estimated wire-length and capacitance](#optimize-placement-using-estimated-wl-cap)
  - [Final placement optimization](final-placement-opt)
  - [Need for libraries and characterization](#need-for-libraries-and-char)
  - [Congestion aware placement using RePlAce](#congestion-aware-placement-using-replace)
- [Cell design and characterization flows](#cell-design-and-char-flows)
  - [Inputs for cell design flow](#inputs-for-cell-design-flow)
  - [Circuit design step](#circuit-design-step)
  - [Layout design step](#layout-design-step)
  - [Typical Characterization flow](#typical-char-flow)
- [General Timing Characterization Parameters](#general-timing-char-parameters)  
  - [Timing threshold definations](#timing-threshold-definations)
  - [Propogation delay and transition time](#propogation-delay-and-transition-time)

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

Utilization Factor = Area occupied by netlist/total core area
![Alt Text](images/6_uti_formula.png)

- In this example, the four blocks completely occupy the core area (4 unit² occupied / 4 unit² total = 1.0 → 100 %).
![Alt Text](images/7_uti_cal.png)

**Note:** Real designs typically target 60–80 % utilization to leave room for routing nets, filler cells, and power straps etc.

<a id="cencept-of-pre--placed-cells"></a>
## Concept of pre-placed cells

<a id="de--coupling-capacitors"></a>
## De-coupling Capacitors

<a id="power-planning"></a>
## Power planning

<a id="pin--placement-and-logical-cell-placement-blockage"></a>
## Pin-placement and logical cell placement blockage

<a id="steps-to-run-floorplan-using-openlane"></a>
## Steps-to-run-floorplan-using-OpenLANE

  
<a id="review-floorplan-files-and-steps-to-view-floorplan"></a>
## Review floorplan files and steps to view floorplan


<a id="review-floorplan-layout-in-magic"></a>
## Review floorplan layout in magin

<a id="library-binding-and-placement"></a>
## Library Binding and Placement


<a id="library-binding-and-placement"></a>
# Library Binding and Placement

<a id="netlist-binidng-and-initial-place-design"></a>
## Netlist binding and initial place design

<a id="optimize-placement-using-estimated-wl-cap"></a>
## Optimize placement using estimated wire-length and capacitance

<a id="final-placement-opt"></a>
## Final placement optimization

<a id="need-for-libraries-and-char"></a>
## Need for libraries and characterization

<a id="congestion-aware-placement-using-replace"></a>
## Congestion aware placement using RePlAce

<a id="cell-design-and-char-flows"></a>
# Cell design and characterization flows

<a id="inputs-for-cell-design-flow"></a>
## Inputs for cell design flow

<a id="circuit-design-step"></a>
## Circuit design step

<a id="layout-design-step"></a>
## Layout design step

<a id="typical-char-flow"></a>
## Typical Characterization flow

<a id="general-timing-char-parameters"></a>
# General Timing Characterization Parameters

<a id="timing-threshold-definations"></a>
## Timing threshold definations

<a id="propogation-delay-and-transition-time"></a>
## Propogation delay and transition time
