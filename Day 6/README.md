<details>
  <Summary><strong> Day 6 : VSDBabySoC Post-Synthesis Simulation</strong></summary>

## Introduction
- Post-synthesis simulation is an essential step in the digital design flow where we verify the functionality and timing of the design after synthesis. While pre-synthesis simulation checks the RTL code for logical correctness, post-synthesis simulation ensures that the synthesized gate-level netlist still behaves as intended.

- In this stage, the RTL is already transformed into a netlist composed of logic gates mapped to a specific technology library. Simulating this netlist helps identify:
  - Functional consistency: Confirms that synthesis has not altered the design's intended behavior.
  - Timing characteristics: Includes realistic gate delays to check for potential timing issues.
  - Synthesis-induced issues: Detects problems like glitches, race conditions, or unintended latches that might not appear in RTL simulation.

- For the VSDBabySoC design, we perform synthesis using Yosys to generate the gate-level netlist, and then simulate this netlist using a same testbench. The goal is to compare the post-synthesis simulation output with the pre-synthesis results. If both match, it validates that the design's logic and functionality are preserved through the synthesis process.

- This step is critical for catching low-level issues early and ensuring the design is ready for downstream physical implementation.

### Synthesis using Yosys

</details>
