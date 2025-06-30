<details>
  <Summary><strong> Day 13 : OpenRoad Installation</strong></summary>

# Contents
- [Steps to Install OpenROAD and Run GUI](#steps-to-install-openroad-and-run-gui)

<a id="steps-to-install-openroad-and-run-gui"></a>
# Steps to Install OpenROAD and Run GUI
- Step1: Clone the OpenRoad Repository
```bash
  git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
  cd OpenROAD-flow-script
```
![Alt Text](images/1.png)

- Step 2: Run the Setup Script
```bash
sudo ./setup.sh
```
![Alt Text](images/2.png)

- Step 3: Build OpenROAD
```bash
./build_openroad.sh --local
```
![Alt Text](images/3.png)

- Step 4: Verify Installation
```bash
source ./env.sh
yosys -help  
openroad -help
```
![Alt Text](images/4_1.png)
![Alt Text](images/4_2.png)

- Step 5: Run the OpenROAD Flow
```bash
cd flow
make
```
![Alt Text](images/5.png)

- Step 6: Launch the graphical user interface (GUI) to visualize the final layout:
```bash
make gui_final
```
![Alt Text](images/6.png)
![Alt Text](images/6_1.png)

OpenRoad tool installation complete! You can now explore the full RTL-to-GDSII flow using OpenROAD.

## OpenRoad Flow Scripts Directory Structure
![Alt Text](images/7.png)

