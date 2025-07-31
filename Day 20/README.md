<details>
  <Summary><strong> Day 20 : Physical Design Flow for VSDBabySoC using OpenROAD</strong></summary>

# Contents
- [`Synthesis`](#syn)
- [`Floorplan`](#fp)
- [`Placement`](#plc)
- [`Clock Tree Synthesis`](#cts)

<a id="syn"></a>
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
```

![Alt Text](images/7.png)
![Alt Text](images/6.png)

```bash
# view synthesiszed netlist
gvim results/sky130hd/vsdbabysoc/base/1_2_yosys.v

# view synthesis log
gvim logs/sky130hd/vsdbabysoc/base/1_2_yosys.log

# view statistics
gvim reports/sky130hd/vsdbabysoc/base/synth_stat.txt
```

**synthesiszed netlist:**
![Alt Text](images/8.png)

**Synthesis log**
![Alt Text](images/9.png)

**Synthesis Stat**
![Alt Text](images/10.png)


<a id="fp"></a>
# `Floorplan`

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk floorplan
```

![Alt Text](images/3.png)
![Alt Text](images/4.png)
![Alt Text](images/5.png)

```bash
# run floorplan and view result with gui
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_floorplan
```

![Alt Text](images/2_floorplan.png)

![Alt Text](images/1_floorplan.png)

<a id="plc"></a>
# `Placement`

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk placement
```

```bash
# run placement and view result with gui
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_placement
```

<a id="cts"></a>
# `Clock Tree Synthesis`

```bash
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts
```

```bash
# run cts and view result with gui
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts
```
