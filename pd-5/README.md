# Module 5 — SKY130 Physical Design: Routing, DRC, Parasitic Extraction & TritonRoute

## 📌 Overview

This module covers the major stages of the physical design flow using the **SKY130** open-source PDK and **OpenLane**-based tools, with a focus on routing and post-routing verification.

**Topics covered:**
- Global and detailed routing
- Maze routing using Lee's Algorithm
- Design Rule Checking (DRC)
- Wire-width and via-spacing verification
- Parasitic extraction & SPEF generation
- TritonRoute detailed routing engine
- Route guide preprocessing
- Intra-layer / inter-layer routing strategies
- Connectivity handling (Access Points & Clusters)
- Routing topology optimization
- OpenLane synthesis and routing results

---

## 🧰 Tools & Technologies

| Tool / Technology | Purpose |
|---|---|
| **OpenLane** | Automated RTL-to-GDSII physical design flow |
| **SKY130** | Open-source 130nm semiconductor process technology |
| **TritonRoute** | Design-rule-aware detailed routing engine |
| **OpenROAD** | Physical design implementation and optimization |
| **OpenSTA** | Static Timing Analysis |
| **Magic** | Layout viewing and physical verification |
| **LEF** | Library Exchange Format (physical library data) |
| **DEF** | Design Exchange Format (physical design data) |
| **SPEF** | Standard Parasitic Exchange Format |
| **Linux/Ubuntu** | Execution environment |
| **Oracle VM VirtualBox** | VM environment for running the Linux-based flow |

---

## 📖 Table of Contents

1. [Introduction](#introduction)
2. [Routing](#1-routing)
3. [Maze Routing – Lee's Algorithm](#2-maze-routing--lees-algorithm)
4. [Design Rule Checking](#3-design-rule-checking)
5. [DRC – Wire Width](#4-drc--wire-width)
6. [DRC – Via Spacing](#5-drc--via-spacing)
7. [Parasitic Extraction](#6-parasitic-extraction)
8. [OpenLane Physical Design Flow](#7-openlane-physical-design-flow)
9. [Floorplanning and Power Planning](#8-floorplanning-and-power-planning)
10. [OpenLane Physical Design Execution](#9-openlane-physical-design-execution)
11. [OpenLane Configuration Parameters](#10-openlane-configuration-parameters)
12. [Routing Output](#11-routing-output)
13. [Fast Route and Detailed Route](#12-fast-route-and-detailed-route)
14. [TritonRoute](#13-tritonroute)
15. [Preprocessed Route Guides](#14-preprocessed-route-guides)
16. [Intra-Layer / Inter-Layer Routing](#15-intra-layer-parallel-and-inter-layer-sequential-routing)
17. [TritonRoute Problem Statement](#16-tritonroute-problem-statement)
18. [Handling Connectivity](#17-handling-connectivity)
19. [Routing Topology Algorithm](#18-routing-topology-algorithm)
20. [OpenLane TritonRoute Execution](#19-openlane-tritonroute-execution)
21. [Routing Data Generation](#20-routing-data-generation)
22. [SPEF / Parasitic Extraction](#21-spef--parasitic-extraction)
23. [OpenLane Synthesis and Routing Results](#22-openlane-synthesis-and-routing-results)
24. [Physical Design Flow Summary](#-physical-design-flow-summary)
25. [Key Learning Outcomes](#-key-learning-outcomes)
26. [Conclusion](#-conclusion)

---

## Introduction

Physical design is a critical stage in the VLSI flow where a synthesized netlist is converted into a manufacturable silicon layout.

This module explores the SKY130 + OpenLane physical design flow, focusing on:
- Global and detailed routing
- Maze routing (Lee's Algorithm)
- DRC, wire-width, and via-spacing checks
- Parasitic extraction and SPEF generation
- Detailed routing with TritonRoute

Practical work also includes route-guide preprocessing, intra-layer parallel routing, inter-layer sequential routing, connectivity handling, topology optimization, and inspection of OpenLane's generated synthesis/routing results.

**Goal:** Understand how routing satisfies design rules, maintains connectivity, and prepares a design for post-layout timing analysis and physical verification.

---

## 1. Routing

Routing connects placed standard cells, pins, and other components through metal layers while satisfying design rules.

Main stages:
- Global routing
- Fast routing
- Detailed routing
- Design-rule-aware routing
- Connectivity verification

### 1.1 Route

Connects input/output pins across metal layers using buffers, flip-flops, and routing tracks.

<img width="1919" height="1070" alt="Screenshot 2026-09-25 145901" src="https://github.com/user-attachments/assets/e29845ce-1be6-4e1f-a062-db7a0e69febf" />

> Routed design showing nets connecting input pins (`Din1–Din4`, `CLK1`, `CLK2`) to output pins. Includes flip-flops, buffers, decap cells, standard cells, metal interconnects, and clock/data paths.

---

## 2. Maze Routing – Lee's Algorithm

A grid-based technique for finding a valid path between two points while avoiding obstacles.

**Algorithm steps:**
1. Start from the source point
2. Expand the routing grid
3. Assign distance values to reachable cells
4. Continue until destination is reached
5. Backtrack along the minimum-distance path
<img width="1917" height="977" alt="Screenshot 2026-09-25 150517" src="https://github.com/user-attachments/assets/0763c98d-1396-4281-a363-c5a3a549adf2" />


> Grid explored and labeled by distance from source. Guarantees shortest valid path when one exists, though memory-intensive for large grids.

---

## 3. Design Rule Checking

DRC verifies that a layout satisfies fabrication rules for the chosen technology.

**Typical rules:**
- Minimum wire width
- Minimum spacing
- Via spacing
- Metal enclosure
- Layer-specific restrictions
- Connectivity constraints

### 3.1 DRC Clean Layout
<img width="995" height="533" alt="image" src="https://github.com/user-attachments/assets/6f5faa4b-3234-451d-bd1e-2c80a855116c" />


> Routed design after DRC verification — a "clean" result means all checked rules are satisfied.

---

## 4. DRC – Wire Width

Wires narrower than the minimum allowed width can violate manufacturing rules and reduce interconnect reliability.

<img width="713" height="337" alt="image" src="https://github.com/user-attachments/assets/949f96bf-549e-40fd-bc0e-f03f0a6912cb" />

> Illustrates the minimum-width constraint applied during verification.

---

## 5. DRC – Via Spacing

Correct spacing between vias and surrounding structures prevents manufacturing violations and unintended shorts.

<img width="829" height="367" alt="image" src="https://github.com/user-attachments/assets/e7ecd903-f2d7-4de8-8427-c7bcbd5deb2c" />

> Shows adequate spacing maintained between vias and nearby structures.

---

## 6. Parasitic Extraction

After routing, parasitic elements from physical interconnects must be extracted:
- Resistance
- Capacitance
- Interconnect delay
- Coupling effects

Used later for accurate post-layout timing analysis.
<img width="1566" height="708" alt="Screenshot 2026-09-25 151753" src="https://github.com/user-attachments/assets/850a1d65-bdc7-40d8-8728-73fac462bca2" />


> Layout after the parasitic extraction stage.

---

## 7. OpenLane Physical Design Flow

```
RTL
 ↓
Synthesis
 ↓
Floorplanning
 ↓
Power Planning
 ↓
Placement
 ↓
CTS
 ↓
Routing
 ↓
DRC / LVS
 ↓
Parasitic Extraction
 ↓
Timing Analysis
 ↓
GDSII
```

<img width="1414" height="871" alt="Screenshot 2026-09-25 155551" src="https://github.com/user-attachments/assets/587871f9-abc4-4b66-8175-aa3f7557fbb7" />

> Execution of the OpenLane flow and generation of technology/design files.

---

## 8. Floorplanning and Power Planning

**Key elements:**
- Core area / Die area
- Standard-cell rows
- I/O placement
- Power rings & straps
- Macro placement

<img width="1151" height="777" alt="Screenshot 2026-09-25 155740" src="https://github.com/user-attachments/assets/60cd33fd-330c-499f-ba46-ef23d86a3ea0" />

> Standard-cell rows, power connections, power stripes, block power ring, I/O and corner pads, macro cell, and block halo. Ensures reliable VPWR/VGND distribution.

---

## 9. OpenLane Physical Design Execution

<img width="957" height="419" alt="Screenshot 2026-09-26 000352" src="https://github.com/user-attachments/assets/2e57afae-3c5c-4320-ae37-a138a30af471" />

> Terminal output showing generated technology data, design data, grid information, and routing-related information.

---

## 10. OpenLane Configuration Parameters

**Key categories:**
- Placement
- Clock Tree Synthesis (CTS)
- Routing
- Magic
- Density
- Timing
- Routing optimization

<img width="1573" height="812" alt="Screenshot 2026-09-25 160424" src="https://github.com/user-attachments/assets/abf2b09f-5173-41f7-8f0a-b2e59fb688e9" />

> Parameters controlling cell density, clock-tree generation, routing layers/optimization, and layout generation.

---

## 11. Routing Output

<img width="1244" height="483" alt="image" src="https://github.com/user-attachments/assets/0737d260-efa8-4b6c-a325-4432b3dffc6c" />

> Generated routing-related files and completion of the routing stage.

---

## 12. Fast Route and Detailed Route

| Stage | Description |
|---|---|
| **Fast Route** | Provides an initial routing solution and generates route guides |
| **Detailed Route** | Converts initial routing info into physical routing satisfying detailed design rules |

<img width="1717" height="941" alt="Screenshot 2026-09-25 161254" src="https://github.com/user-attachments/assets/38a577d8-2276-4ea8-94cc-635786a209b9" />

---

## 13. TritonRoute

A detailed routing engine considering:
- Routing guides
- Design rules
- Connectivity
- Metal layers
- Via placement
- Routing constraints

<img width="1211" height="504" alt="Screenshot 2026-09-25 161333" src="https://github.com/user-attachments/assets/8fe19d1f-42ef-45c9-b56b-96e9176eda0a" />

> TritonRoute performs initial detailed routing while following preprocessed route guides.

---

## 14. Preprocessed Route Guides

Route guides indicate preferred regions/directions for net routing.

**Requirements:**
- Route guides should have unit width
- Route guides should follow the preferred routing direction

<img width="1041" height="687" alt="Screenshot 2026-09-25 161322" src="https://github.com/user-attachments/assets/bd84730e-222e-4c5e-9d66-4e4ca5d38696" />


---

## 15. Intra-Layer Parallel and Inter-Layer Sequential Routing

| Strategy | Description |
|---|---|
| **Intra-Layer Parallel** | Multiple routing tasks handled in parallel within the same metal layer |
| **Inter-Layer Sequential** | Routing proceeds sequentially across layers for full connectivity |

<img width="1141" height="610" alt="Screenshot 2026-09-25 161749" src="https://github.com/user-attachments/assets/16a44f1e-8ada-4b1a-a83f-21c3fe79a4ce" />

> Panel-based routing involving intra-layer parallel routing, inter-layer sequential routing, multiple metal layers, and panel decomposition.

---

## 16. TritonRoute Problem Statement

**Inputs:**
- LEF
- DEF
- Preprocessed route guides

**Output:** Detailed routing solution optimized for wire length and via count

**Constraints:**
- Route-guide constraints
- Connectivity constraints
- Design rules

<img width="709" height="380" alt="image" src="https://github.com/user-attachments/assets/c612772a-7c34-468e-9832-94a6ab4dc0a7" />


---

## 17. Handling Connectivity

- **Access Point (AP):** An on-grid point on a route guide's metal layer, used to connect lower-layer segments, upper-layer segments, pins, or I/O ports.
- **Access Point Cluster (APC):** A union of access points derived from the same lower-layer segment, upper-layer guide, pin, or I/O port.

<img width="1193" height="628" alt="Screenshot 2026-09-25 161937" src="https://github.com/user-attachments/assets/d413b5b0-e767-476d-85d3-5ad44a498252" />

---

## 18. Routing Topology Algorithm

Determines how multiple connection points of a net are connected efficiently, minimizing routing cost while maintaining full connectivity.


<img width="1094" height="502" alt="Screenshot 2026-09-25 162240" src="https://github.com/user-attachments/assets/a792b296-761b-4244-ab36-bfb1e6eaade9" />

> Algorithm evaluates connectivity and cost to produce an optimized routing tree.

---

## 19. OpenLane TritonRoute Execution


<img width="924" height="453" alt="image" src="https://github.com/user-attachments/assets/39fc29cf-379c-4552-ae26-445907751252" />

> Execution environment and OpenLane working directories used for the flow.

---

## 20. Routing Data Generation

<img width="1912" height="920" alt="Screenshot 2026-09-25 163345" src="https://github.com/user-attachments/assets/6a9b0638-6876-4e70-b6f2-8a2e2ce2f093" />

> Generated routing data and related files stored in OpenLane run directories for further analysis and verification.

---

## 21. SPEF / Parasitic Extraction

SPEF represents extracted parasitic information, including:
- Resistance
- Capacitance
- Nets
- Interconnect parasitics
- Connectivity

<img width="1129" height="539" alt="image" src="https://github.com/user-attachments/assets/e30efee2-181b-4a50-bfe3-f6af95fd8477" />

> Extraction script processes the routed DEF and generates parasitic information for post-layout analysis.

---

## 22. OpenLane Synthesis and Routing Results

```
results/
├── synthesis/
├── routing/
├── placement/
├── cts/
├── floorplan/
└── signoff/
```

<img width="1089" height="570" alt="image" src="https://github.com/user-attachments/assets/d9cd8d80-361e-4c70-a07f-acee4cee2d70" />

> Run directories and generated synthesis/routing results, used to verify completion of each stage.

---

## 🔄 Physical Design Flow Summary

```
RTL Design
 ↓
Synthesis
 ↓
Floorplanning
 ↓
Power Planning
 ↓
Placement
 ↓
Clock Tree Synthesis
 ↓
Global Routing
 ↓
Fast Route
 ↓
Route Guide Generation
 ↓
Route Guide Preprocessing
 ↓
Detailed Routing
 ↓
TritonRoute
 ↓
DRC Verification
 ↓
Parasitic Extraction
 ↓
SPEF Generation
 ↓
Timing Analysis
 ↓
Physical Verification
```

---

## ✅ Key Learning Outcomes

- Understanding physical design routing
- Understanding maze routing and Lee's Algorithm
- Understanding global and detailed routing
- Understanding fast routing and detailed routing
- Understanding TritonRoute
- Understanding route guides and route-guide preprocessing
- Understanding intra-layer and inter-layer routing
- Understanding access points and access point clusters
- Understanding routing topology optimization
- Understanding DRC verification
- Understanding wire-width and via-spacing constraints
- Understanding parasitic extraction
- Understanding SPEF generation
- Understanding OpenLane routing results
- Understanding OpenLane physical-design directories and outputs

---

