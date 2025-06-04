<details>
  <Summary><strong> Day 6 : Timing Graphs using OpenSTA</strong></summary>

## 📚 Contents

## Introduction to STA
Static Timing Analysis (STA) is a crucial method in digital design used to verify the timing performance of a circuit without requiring simulation or input stimulus. It checks all possible paths in a design for timing violations, ensuring that signals propagate within acceptable time limits and meet the design’s setup and hold requirements.

Unlike dynamic timing analysis, which simulates input vectors and observes the behavior over time, STA is static, it does not depend on input values or functional simulation. This makes it extremely fast and exhaustive, making it the industry standard for sign-off timing verification in ASIC and SoC flows.

## OpenSTA and Installation

OpenSTA is an open-source gate-level Static Timing Analysis tool developed by Parallax Software. 

- You can install OpenSTA using two different methods:
  - Native Installation with Local CUDD: This method involves installing OpenSTA directly on your system using a manually built CUDD.
  - Docker-based Installation: This method involves installing OpenSTA inside a Docker container, which can be self-contained and clean.

### 🔹 Method 1: Native Installation with Local CUDD
This method provides full control and is suitable for script automation.

#### Steps:

##### Step 1: Install prerequisites:
  
```bash
sudo apt update
sudo apt install -y build-essential cmake git \
  tcl-dev swig bison flex zlib1g-dev libeigen3-dev
```
  
##### Step 2: Build and install CUDD:
  
```bash
wget https://github.com/davidkebo/cudd/raw/main/cudd_versions/cudd-3.0.0.tar.gz
tar -xvzf cudd-3.0.0.tar.gz
cd cudd-3.0.0
./configure --prefix=$HOME/cudd
make -j$(nproc)
make install
cd ..
```
![Alt Text](images/step2_cmake.png)
![Alt Text](images/step2_make.png)
  
##### Step3: Build OpenSTA with CMake:
  
  ```bash
  git clone https://github.com/parallaxsw/OpenSTA.git
  cd OpenSTA
  mkdir build && cd build
  cmake -DCUDD_DIR=$HOME/cudd ..
  make -j$(nproc)
  ./sta
  ```

![Alt Text](images/OpenSTA_with_CUDD.png)

### 🔹 Method 2: Docker-based Installation
This method offers a clean, isolated, ready-to-use environment.

#### Steps:

##### Step 1: Install Docker on Ubuntu
```bash
# 1. Remove any older Docker versions (optional)
sudo apt remove docker docker-engine docker.io containerd runc

# 2. Update and install prerequisites
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release

# 3. Add Docker’s official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 4. Set up the Docker stable repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Install Docker Engine
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

##### Step 2: Start Docker
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

##### Step 3: Verify Docker is working
```bash
sudo docker run hello-world
```

- This should print a "Hello from Docker!" message confirming Docker is installed correctly.
![Alt Text](images/s3_verify_docker_is_working.png)

##### Step 4: Clone the OpenSTA Repository
```bash
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
```

##### Step 5: Build the OpenSTA Docker Image
```bash
sudo docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```
- This will take a few minutes and install all dependencies (including CUDD) inside the Docker image.

#### Step 6: Run OpenSTA from Docker
```bash
sudo docker run -it -v $HOME:/data opensta
```
Here,
- -it: interactive terminal
- -v $HOME:/data: mounts your home directory inside the container so you can access files

![Alt Text](images/s6.png)

Once inside, you’ll see the sta> prompt — you're ready to use OpenSTA.

## Timing Analysis using In line Commands
- Basic timing analysis using in-line commands within OpenSTA shell (%).

```bash
# Load the Liberty timing library (standard cell delays, arcs, etc.)
read_liberty /OpenSTA/examples/nangate45_slow.lib.gz

# Read the synthesized gate-level Verilog netlist
read_verilog /OpenSTA/examples/example1.v

# Set the top-level module of the design (as defined in the Verilog)
link_design top

# Create a clock named 'clk' with a 10 ns period, connected to clk1, clk2, and clk3
create_clock -name clk -period 10 {clk1 clk2 clk3}

# Define input delays of 0 ns for inputs in1 and in2, relative to the clk
set_input_delay -clock clk 0 {in1 in2}

# Report any timing violations (setup/hold) across the design
report_checks
```

![Alt Text](images/example1_slow_lib_report.png)

- The report shows analysis for a <strong> maximum delay path (i.e setup check)</strong> from register `r2` to `r3` on the clock `clk`.
- The default behavior of the `report_checks` in OpenSTA is to report maximum delay paths, unless explicitly asked for minumum (hold) analysis.
- The path starts at the <strong> Q output of reg r2</strong> (a DFF) and the path ends at the <strong> D input of reg r3</strong> (another DFF).

##### Delay Breakdown
###### Arrival Time
Delay    Time     Description

###### Required Time



![Alt Text](images/example1_design.png)





