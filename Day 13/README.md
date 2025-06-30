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
