<details>
  <Summary><strong> Day 18 : Pre-layout Timing Analysis and Importance of Good Clock Tree</strong></summary>

# Contents
- [Fix DRC errors and verify the design](#fix-drc-errors-and-verify-the-design)
- [Save the final layout with custom name and open it](#save-final-layout)
- [Generate lef from the Layout](#generate-lef-from-the-layout)


<a id="fix-drc-errors-and-verify-the-design"></a>
# Fix up small DRC errors and verify the design is ready to be inserted into our flow

Before moving forward with custom designed cell layout verify following:
1. The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks.
2. Width of the standard cell should be odd multiples of the horizontal track pitch.
3. Height of the standard cell should be even multiples of the vertical track pitch.

**Open custom inverter layout**
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

![Alt Text](images/1.png)


### Convert Grid info to track info

- press **g** in magic to activate grids.

**tracks.info of sky130_fd_sc_hd:**
![Alt Text](images/2.png)

- Commands to set grid as tracks of locali layer:

```bash
# Get syntax for grid command
help grid

# Set grid values accordingly
grid 0.46um 0.34um 0.23um 0.17um
```

![Alt Text](images/3.png)

**Verified ✅ --> The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks**
![Alt Text](images/4.png)

**Verified ✅ --> Width of the standard cell should be odd multiples of the horizontal track pitch**
Horizontal track pitch = 0.46 µm
Width of standard cell = 1.38 µm = 0.46 x 3
![Alt Text](images/5.png)

**Verified ✅ --> Height of the standard cell should be even multiples of the vertical track pitch**
Vertical track pitch = 0.34 µm
Height of standard cell = 2.72 µm = 0.34 × 8
![Alt Text](images/6.png)

<a id="save-final-layout"></a>
# Save the final layout with custom name and open it

```bash
# Command to save as
save sky130_vsdinv.mag

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_vsdinv.mag &
```

**newly saved layout**
![Alt Text](images/7.png)


<a id="save-final-layout"></a>
# Generate lef from the Layout

```bash
# lef command
lef write

#open newly created lef file
gvim sky130_vsdinv.lef
```

![Alt Text](images/8.png)

**lef file**
![Alt Text](images/9.png)


# Copy the newly generated lef and associated required lib files to 'picorv32a' design 'src' directory



