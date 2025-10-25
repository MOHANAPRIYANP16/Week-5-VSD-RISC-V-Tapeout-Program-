# Routing and Static Timing Analysis of GCD  

This lab focuses on using **OpenROAD Flow Scripts** to carry out the **Routing** and **Static Timing Analysis (STA)** stages for the **GCD** circuit. It demonstrates the continuation from floorplanning and placement to **signal routing** and **timing verification** — crucial steps in the **VLSI physical design flow**.

---

## Table of Contents

1. [Routing and Static Timing Analysis of GCD](#routing-and-static-timing-analysis-of-gcd)  
2. [Routing of `gcd`](#routing-of-gcd)  
   - [Global and Detailed Routing](#global-and-detailed-routing)  
3. [Static Timing Analysis (STA) of `gcd`](#static-timing-analysis-sta-of-gcd)  
4. [Summary](#summary)  

---

## Importance of Floorplanning and Placement

**Floorplanning** is the process of defining the **physical layout of a chip** before placing standard cells. It involves:  

- Allocating **die and core area**.  
- Placing **I/O pads** around the die periphery.  
- Arranging **macros** (like SRAM, PLLs) strategically.  
- Dividing the core into **standard cell rows**.  
- Designing a robust **power delivery network (PDN)**.  
- Reserving areas for **physical-only cells** (tap cells, endcaps, blockages).  

**Placement** is the step where **standard cells and small blocks** are arranged within the core area defined by floorplanning. The goals are:  

- **Minimize wire length** between connected cells.  
- **Reduce congestion** for easier routing.  
- **Meet timing constraints** to ensure proper circuit operation.  

---

## Installation of OpenROAD Flow

Before setting up **OpenROAD Flow Scripts**, ensure the following requirements are met:

- **Operating System:** Ubuntu 20.04 or later (preferably Ubuntu 22.04) [I'm using `Zorin OS` which is based on Ubuntu 22.04]
- **Git:** Required for cloning repositories.
- **Build Tools:** Includes CMake, GCC/G++, Make, and standard development libraries needed for compiling the flow.
- **Python 3:** Version 3.6 or higher is recommended.
- **EDA Tools:**
  - **OpenROAD** – for placement and routing.
  - **Yosys** – for logic synthesis.
  - **OpenSTA** – for static timing analysis.


### Installation of OpenROAD

Firstly, `or-tools` needs to be installed,

```bash
git clone https://github.com/google/or-tools.git
cd or-tools
```


then, configure the build

```bash
cmake -S . -B build -DBUILD_DEPS=ON
```

and install the `or-tools` using,

```bash
cmake --build build --config Release --target install -v
```

This will take an hour or more be warned.

![ortools](Images/ortools.png)

---

Then to set OpenROAD up,

- Clone the official **OpenROAD repository**.

```bash
git clone --recursive git clone https://github.com/The-OpenROAD-Project/OpenROAD.git
cd OpenROAD
```

- Initialize and update its submodules to include all dependencies.

```bash
git submodule update --init --recursive
```

- Compile the source code to generate the OpenROAD binaries.

```bash
rm -rf build
mkdir build
cd build
sudo cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_CXX_FLAGS="-Wno-error" -DCMAKE_PREFIX_PATH="/usr/local" -DCMAKE_CXX_COMPILER=/usr/bin/g++-9
```

This too will take more than an hour.

![alt text](openrd_sta_verify.png)

---

### Installation of Flow Scripts

Clone the [OpenROAD Flow Scripts repo](https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts) inside a folder of your choice

```bash 
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
```
![alt text](flowscripts_openrd.png)

Then to map the installed OpenROAD, and previously installed OpenSTA and Yosys, simply following the commands below,

```bash
export OPENROAD_EXE=/usr/local/bin/openroad
export YOSYS_EXE=~/Documents/Apps/oss-cad-suite/bin/yosys
export OPENSTA_EXE=/usr/local/bin/sta
```

> [!Tip]
> Use `whereis` command to locate the binaries of OpenROAD, Yosys and OpenSTA


With this OpenROAD and its script flow installation is complete.

---

## Floorplan of `gcd` 

Navigate to the `flow` directory inside `OpenROAD-flow-scripts` 

This is where the flow of our required module `gcd` will take place 

To run floorplan of this module, follow the below commands 

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk floorplan
```

![alt text](floorplan.png)

The die size and core utilization percentage are observed in the OpenROAD flow scripts, ensuring that the floorplan and placement adhere to the specified physical constraints.

And then, to view the gui,

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk gui_floorplan
```

![alt text](gui1.png)

---

## Placement of `gcd`

To run placement for this module, use the following command:  

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk place
```  

![placement](Images/placement.png)

This will generate the placement for the module.  

To view the placement in the GUI, run:  

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk gui_place
```




---

### STA Output Includes:
- **Worst Negative Slack (WNS)**  
- **Total Negative Slack (TNS)**  
- **Path Delay Reports**  
- **Setup and Hold Timing Margins**

---

## Summary

This lab extends the **physical design process** for the **GCD module** to include **Routing** and **Static Timing Analysis (STA)** using **OpenROAD Flow Scripts**.  
It demonstrates the completion of interconnections and timing verification — essential steps toward a manufacturable chip layout.

### Key Steps Covered:

#### Routing
- Performed **Global** and **Detailed Routing**.  
- Generated interconnections between placed cells.  
- Verified routing completion through GUI visualization.  

#### Static Timing Analysis
- Used **OpenSTA** to analyze timing after routing.  
- Reviewed setup, hold, and delay results.  
- Verified timing closure by checking slack values.  

---

## Outcome

By completing this experiment, the **GCD design** has successfully transitioned from a **placed layout** to a **fully routed and timing-verified circuit**.  
This marks a major milestone before performing **DRC/LVS checks** and **power analysis** in the next stages of the VLSI backend flow.
