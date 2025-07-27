<details>
  <Summary><strong> Day 19: Final Steps for RTL2GDS using tritonRoute and openSTA</strong></summary>

# Contents
- [Routing & Design Rule Check (DRC)](#routing-and-drc)
  - [Maze Routing and Lee’s Algorithm](#maze-routing)
  - [Design Rule Check](#drc)
- [Step 14: Perform detailed routing using TritonRoute and explore the routed layout](#detailed-routing-using-tritonroute)
- [Step 15: Post-Route parasitic extraction using SPEF extractor](#spef)
- [Step 16: Post-Route OpenSTA timing analysis with the extracted parasitics of the route](#post-route-opensta)

<a id="routing-and-drc"></a>
# Routing & Design Rule Check (DRC)
<a id="maze-routing"></a>
## Maze Routing and Lee’s Algorithm

- Routing is the process of determining the optimal path to connect two circuit elements, such as clocks, flip-flops, or logic gates.
- Several routing strategies exist, including the **Steiner Tree Algorithm** and the **Line Search Algorithm**. One of the fundamental routing techniques is Lee's **Maze Routing** Algorithm (Lee, 1961).

![Alt Text](images/1.png)
![Alt Text](images/2.png)

- Consider a scenario where two points, **source** and **target**, need to be connected. The goal is to determine the shortest and most efficient path, avoiding excessive bends or zig-zags, while favoring L-shaped connections.
- From an algorithmic perspective, the software needs to explore and determine this route, whereas from a physical design standpoint, this path represents the actual metal wire that facilitates signal transmission.
- **Lee’s Algorithm** is widely utilized in **grid-based routing**, making it well-suited for integrated circuit (IC) design. It systematically finds a path in a **maze-like grid** using a wave-expansion method.

### Steps in Lee’s Algorithm:
- `Initialization:` The algorithm begins by setting up a **routing grid** or **matrix** over the routing area. Each grid cell is classified as one of the following:
  - Source (S) - starting point
  - Target (T) - destination to be reached
  - Obstacle - blocked areas like macros, hard IPs
  - Empty space - available for routing
  - Visited cell - explored during wave propagation

![Alt Text](images/3.png)

- `Wave Expansion:` The algorithm starts at the source cell (S) and expands outward in all directions. Neighboring cells (up, down, left, and right) are examined, and each newly visited cell is assigned a value one greater than the lowest neighboring cell (excluding obstacles). This expansion continues until it reaches the target (T) or no further movement is possible.

![Alt Text](images/4.png)
![Alt Text](images/5.png)

- `Backtracking & Path Reconstruction:` Once the target is reached, the algorithm traces back through the cell values to reconstruct the shortest route to the source. If multiple paths exist, the algorithm selects the one with the fewest bends for an optimized connection. The final path follows a non-diagonal approach, ensuring it does not overlap any obstacles such as macros or HIPs.

![Alt Text](images/6.png)

**best possible route to connect source and target**
![Alt Text](images/7.png)

**Limitations:**
- Requires large memory for dense layout.
- Slow

<a id="drc"></a>
## Design Rule Check 
- Routing is not simply about connecting two points—it must also adhere to specific design rules to ensure manufacturability and reliability.
- For example, certain rules specify:
  - **Minimum wire spacing** between two adjacent interconnects.
  - **Minimum wire width and pitch** for different metal layers.
  - **Via placement constraints** such as via width, via spacing, and metal layer hierarchy (higher metal layers should be wider than lower layers).

**Why is DRC Important?**
- Ensures the design can be fabricated correctly on silicon.
- Prevents critical errors such as signal shorts, which can cause functional failures.
- If a signal short is detected, the route can be adjusted by moving it to a different metal layer, though this can introduce additional DRC challenges.

<a id="detailed-routing-using-tritonroute"></a>
# Step 14: Perform detailed routing using TritonRoute and explore the routed layout
**Commands to perform routing:**
```bash
# Check value of 'CURRENT_DEF'
echo $::env(CURRENT_DEF)

# Check value of 'ROUTING_STRATEGY'
echo $::env(ROUTING_STRATEGY)

# Command for detailed route using TritonRoute
run_routing
```

![Alt Text](images/9_run_routing.png)

![Alt Text](images/10_routing_done.png)

**Commands to load routed def in magic in another terminal:**
```bash
# Change directory to path containing routed def
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-07_23-12/results/routing

# Command to load the routed def in magic tool
magic -T /home/sdudigani/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &
```

![Alt Text](images/11_routing_def.png)

![Alt Text](images/12.png)
![Alt Text](images/13.png)
![Alt Text](images/14.png)
![Alt Text](images/15.png)
![Alt Text](images/16_via1.png)
![Alt Text](images/17_metal3.png)

**fast route guide present in `openlane/designs/picorv32a/runs/25-07_23-12/tmp/routing` directory:**
![Alt Text](images/18_fast_route_guide.png)

<a id="spef"></a>
# Step 15: Post-Route parasitic extraction using SPEF extractor
**Commands for SPEF extraction Post-Route parasitic extraction using SPEF extractor:**
```bash
cd ~/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/scripts/spef_extractor

python3 main.py -l /home/sdudigani/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-07_23-12/tmp/merged.lef -d /home/sdudigani/soc-design-and-planning-nasscom-vsd/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/25-07_23-12/results/routing/picorv32a.def
```

![Alt Text](images/19_spef_generation_done.png)

**Contents of extracted spef:**
![Alt Text](images/20_spef.png)

<a id="post-route-opensta"></a>
# Step 16: Post-Route OpenSTA timing analysis with the extracted parasitics of the route

![Alt Text](images/name.png)
![Alt Text](images/name.png)
![Alt Text](images/name.png)


</details>
