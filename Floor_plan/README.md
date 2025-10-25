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

## Routing of `gcd`

After completing **Floorplan** and **Placement**, the next stage is **Routing**, where signal connections between cells are physically established using metal layers.  
Routing is typically divided into two major phases: **Global Routing** and **Detailed Routing**.

Navigate to the `flow` directory inside `OpenROAD-flow-scripts` and execute the routing command as follows:

```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk route
```

This initiates both **Global** and **Detailed Routing**, ensuring all interconnections between placed cells are completed.

To visualize the routing layout in GUI, use:
```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk gui_route
```
---

## Global and Detailed Routing

- **Global Routing:** Divides the chip area into grids and finds approximate routes for all nets.  
- **Detailed Routing:** Converts the global paths into exact wire geometries that follow DRC (Design Rule Check) constraints.

---

## Static Timing Analysis (STA) of `gcd`

Once routing is complete, **Static Timing Analysis (STA)** is performed to verify that the design meets timing requirements such as setup and hold times.

To execute STA using **OpenROAD’s integrated OpenSTA**, run:
```bash
make DESIGN_CONFIG=./designs/sky130hd/gcd/config.mk sta
```
This command will generate timing reports for the post-route design.

To open STA manually within the OpenROAD environment:
```bash
openroad
read_db ./results/sky130hd/gcd/routing/gcd.odb
read_sdc ./designs/sky130hd/gcd/constraint.sdc
report_checks -path_delay min_max
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
