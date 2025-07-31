<details>
  <Summary><strong> Day 20 : Physical Design Flow for VSDBabySoC using OpenROAD</strong></summary>

# Contents
- [`Synthesis`](#synthesis)

# `Synthesis`

<details>
  <Summary><strong>   config.mk contents</strong></summary>
</details>


```bash

cd OpenROAD-flow-scripts/flow
source env.sh
cd flow

# remove any previously generated results, logs, and intermediate files
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk clean_all

# Run Synthesis
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk synth

# view synthesiszed netlist
gvim results/sky130hd/vsdbabysoc/base/1_1_yosys.v

# view synthesis log
gvim logs/sky130hd/vsdbabysoc/base/1_1_yosys.log

# view statistics
gvim reports/sky130hd/vsdbabysoc/base/synth_stat.txt
```

# `Floorplan`

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

```bash
# run floorplan and view result with gui
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

# `Placement`

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk placement
```

```bash
# run placement and view result with gui
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_placement
```

# `Clock Tree Synthesis`

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

```bash
# run cts and view result with gui
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```
