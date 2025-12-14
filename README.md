# NASSCOM-VSD-SoC--design-and--Planning-Program
Digital VLSI Soc Design and Planning Training

## VLSI SoC Design and OpenLane Flow – Workshop Documentation

This document summarizes the concepts and practical work completed during the VLSI SoC Design and Planning workshop. The program focused on understanding how a digital hardware description written in RTL is transformed into a manufacturable GDSII layout using the OpenLane open-source ASIC design flow. The workshop covered both theoretical foundations and hands-on implementation steps.

# DAY1

## Overview of RTL to GDSII Flow

The RTL to GDSII flow describes the complete transformation of a behavioral hardware description into its physical layout. This process includes synthesis, floor planning, placement, clock tree construction, routing, and a set of verification checks. The objective of the flow is to ensure that the final layout accurately reflects the intended logical behavior, meets timing requirements, and satisfies foundry design rules.

## Design Abstraction and VLSI Y-Chart

One of the key concepts introduced was the design abstraction in VLSI. The Y-Chart model illustrates how design progresses through three major domains—behavioral, structural, and geometric. This relationship is essential in understanding how high-level descriptions eventually become physical mask layers used for fabrication.

Chip Components: Package and Internal Structure

## A detailed discussion was provided on the internal composition of an integrated circuit. The following components were studied:

### 1. Core

The region that contains the logic cells, IP blocks, combinational and sequential circuits, and internal routing resources.

### 2. Die

The physical silicon area containing the core and all peripheral structures. Multiple dies are fabricated on a wafer and later separated.

### 3. I/O Pads

Interfaces that connect on-chip signals to the external package. These include input, output, bidirectional, and power pads.

### 4. IP Blocks

Pre-designed modules such as SRAMs, PLLs, ADCs, and DACs. These blocks often require specialized design efforts and are provided by foundries.

### 5. PDK (Process Design Kit)

A PDK acts as the communication layer between EDA tools and the fabrication process. It includes device models, rule files for DRC/LVS, physical layer definitions, and standard cell libraries.
In this workshop, the SkyWater 130 nm PDK (sky130_fd_sc_hd) was used.

# Introduction to RISC-V

- RISC-V is an open standard Instruction Set Architecture (ISA).
- The ISA defines how software instructions are interpreted by hardware.
-  Key points covered:
-  Programming languages are converted to machine-level instructions through a software stack involving :
  1. OS
  2. Compiler
  3. assembler.

The RISC-V ISA is modular, extensible, and fully open, making it suitable for academic and industrial processor development.

Software to Hardware Execution Flow

An overview of how application software interacts with hardware was presented:

Application Layer – user programs

Operating System – manages resources and translates high-level operations into system-level code

Compiler – converts source code to machine-specific instructions

Assembler – maps instructions to binary format understood by hardware

This establishes the connection between software execution and the underlying processor architecture.

Open-Source ASIC Design Stack

The workshop emphasized the three primary components required for a complete ASIC design environment:

RTL Design Files

EDA Tools (Yosys, OpenROAD, Magic, Netgen, KLayout)

PDK Data

OpenLane integrates these tools into a unified flow, enabling end-to-end digital design using open-source resources.

## Simplified RTL to GDS Flow
### 1. Synthesis

The RTL description is translated into a gate-level netlist using standard cells. Cells are provided in multiple representations such as Verilog, Liberty (.lib), LEF, SPICE, and GDS.

### 2. Floor Planning and Power Planning

Defines the layout structure, core area, pin placement, macro positioning, and the power distribution network (power rings, straps, and rails).

### 3. Placement

Standard cells are arranged in rows.

Global Placement provides an approximate location.

Detailed Placement refines positions to ensure alignment and legality.

### 4. Clock Tree Synthesis

A balanced clock network is synthesized to minimize skew across all sequential elements. Structures such as H-trees or X-trees may be used.

### 5. Routing

Global routing generates routing guides, and detailed routing creates the final metal connections according to PDK constraints.

### 6. Sign-off

The physical design undergoes several final checks:

DRC (Design Rule Check)

LVS (Layout vs Schematic)

Parasitic extraction and STA

Antenna rule checks

Antenna prevention techniques such as bridging and diode insertion were discussed. OpenLane utilizes fake diode insertion initially and replaces them with real diodes if violations are detected after routing.

Detailed OpenLane Flow

The complete OpenLane flow includes:

-- RTL Synthesis (Yosys, ABC)

-- Static Timing Analysis

-- Optional DFT insertion using FAULT

-- Floorplan and Powerplan generation

-- Placement and post-placement optimization

-- CTS

-- Routing

-- Physical Verification (Magic, Netgen)

-- GDSII export

The flow ensures functional correctness and manufacturability at each stage.

## Lab Work – Day 1

Hands-on tasks performed include:

Setting up the OpenLane working environment

Running synthesis for sample RTL designs

Inspecting standard cell libraries

Observing floorplan parameters

Opening layout files using Magic

Understanding basic configuration files used in each stage
# COMMANDS USED
![Terminal screenshot](Screenshot%20from%202025-12-12%2011-25-56.png)

![Screenshot 11-28-13](Screenshot%20from%202025-12-12%2011-28-13.png)

![Screenshot 11-34-17](Screenshot%20from%202025-12-12%2011-34-17.png)

![Screenshot 11-38-33](Screenshot%20from%202025-12-12%2011-38-33.png)

![Screenshot 11-38-57](Screenshot%20from%202025-12-12%2011-38-57.png)

## SYNTHESIS SUCCESSFUL 
![Screenshot 11-39-03](Screenshot%20from%202025-12-12%2011-39-03.png)



## Day 2: Floorplanning Concepts and Standard Cell Placement

## Overview
Day 2 of the workshop focused on chip floorplanning fundamentals, understanding good versus bad floorplans, and learning the basics of library cells and placement. The session combined conceptual learning with hands-on implementation using the OpenLANE ASIC flow and layout visualization through MAGIC.

---

## Good Floorplan vs Bad Floorplan
Floorplanning is a critical step in ASIC design as it directly impacts area utilization, routing quality, power distribution, and timing closure. A good floorplan enables smooth routing and scalability, while a bad floorplan can cause congestion, timing violations, and design inefficiencies.

Characteristics of a good floorplan:
- Balanced utilization
- Adequate whitespace for routing
- Proper aspect ratio
- Efficient I/O and power structure placement

---

## Chip Floorplanning Fundamentals

### Core
The core is the central region of the chip where all logic elements such as standard cells, macros, and IP blocks are placed. This area contains the functional logic of the design.

### Die
The die surrounds the core and includes I/O pads, power pads, and boundary regions. Multiple dies are fabricated on a single silicon wafer to improve manufacturing efficiency.

The core dimensions depend on the design netlist, while the die dimensions are derived from the core with additional margins.

---

## Utilization Factor
The utilization factor represents how much of the core area is occupied by logic.
Utilization Factor = (Area occupied by netlist) / (Total core area)

### Observations
- A utilization factor of 1 indicates a poor floorplan.
- No space is left for routing or future design changes.
- Practical designs maintain utilization less than 1.

---

## Aspect Ratio
The aspect ratio defines the shape of the core.
Aspect Ratio = (Height of core) / (Width of core)

### Interpretation
- Aspect ratio equal to 1 represents a square core.
- Aspect ratio not equal to 1 represents a rectangular core.
- A suitable aspect ratio helps reduce congestion and improves placement efficiency.

---

## Floorplanning Examples

### Case 1: Fully Utilized Core
- Core area equals netlist area
- Utilization factor equals 1
- Aspect ratio equals 1
- Considered a poor floorplan due to lack of routing space

### Case 2: Optimized Core Area
- Core area larger than netlist area
- Utilization factor less than 1
- Rectangular core
- Allows better routing and future expansion

---

## Day 2 Labs: Floorplanning Using OpenLANE

### Running the Floorplan
The floorplanning stage was executed using the OpenLANE command:

![Screenshot 11-40-00](Screenshot%20from%202025-12-12%2011-40-00.png)

![Screenshot 11-40-42](Screenshot%20from%202025-12-12%2011-40-42.png)

![Screenshot 11-56-00](Screenshot%20from%202025-12-12%2011-56-00.png)

![Screenshot 12-26-48](Screenshot%20from%202025-12-12%2012-26-48.png)

![Screenshot 12-51-14](Screenshot%20from%202025-12-12%2012-51-14.png)

![Screenshot 12-51-31](Screenshot%20from%202025-12-12%2012-51-31.png)

![Screenshot 12-52-31](Screenshot%20from%202025-12-12%2012-52-31.png)

![Screenshot 13-25-45](Screenshot%20from%202025-12-12%2013-25-45.png)

![Screenshot 13-28-41](Screenshot%20from%202025-12-12%2013-28-41.png)

# Day 3: Inverter Characterization, Library Cells, and Routing Basics

## Overview
Day 3 of the workshop focused on standard cell design and characterization using Sky130 model files. The session covered CMOS inverter simulation using ngspice, extraction of static and dynamic parameters, creation of layout views using MAGIC, and an introduction to LEF files, routing tracks, and DRC debugging.



## CMOS Inverter Simulation Using ngspice
A CMOS inverter was simulated using Sky130 device models to understand both static and dynamic behavior. DC and transient analyses were performed using ngspice.



## Static Characteristics of CMOS Inverter

### Switching Threshold
The input voltage at which the inverter output switches from logic high to logic low.

### Input Low Voltage
The maximum input voltage interpreted as logic 0.

### Input High Voltage
The minimum input voltage interpreted as logic 1.

### Output Low Voltage
The output voltage level corresponding to logic 0.

### Output High Voltage
The output voltage level corresponding to logic 1.

### Noise Margins
Noise margins represent the tolerance of the inverter to noise and are defined for both logic high and logic low regions.



## Dynamic Characteristics of CMOS Inverter

### Propagation Delay
The delay between a change in input and the corresponding change in output.

### Rise Time
The time taken by the output to transition from low to high.

### Fall Time
The time taken by the output to transition from high to low.


## Standard Cell Design Using MAGIC

### Inverter Layout Creation
The CMOS inverter layout was created using the MAGIC layout editor following Sky130 design rules. Proper placement of transistors, diffusion, poly, and metal layers was ensured based on the schematic.


## Parasitic Extraction

### Extraction Process
Once the layout was completed, parasitic extraction was performed to capture resistive and capacitive effects.


# Day 3 Labs: Inverter Characterization

![Screenshot 16-02-46](Screenshot%20from%202025-12-12%2016-02-46.png)

![Screenshot 16-02-55](Screenshot%20from%202025-12-12%2016-02-55.png)

![Screenshot 16-06-20](Screenshot%20from%202025-12-12%2016-06-20.png)

![Screenshot 16-18-34](Screenshot%20from%202025-12-12%2016-18-34.png)

![Screenshot 16-18-38](Screenshot%20from%202025-12-12%2016-18-38.png)

![Screenshot 11-44-09](Screenshot%20from%202025-12-13%2011-44-09.png)

![Screenshot 11-49-39](Screenshot%20from%202025-12-13%2011-49-39.png)

![Screenshot 12-16-57](Screenshot%20from%202025-12-13%2012-16-57.png)

![Screenshot 12-54-55](Screenshot%20from%202025-12-13%2012-54-55.png)

![Screenshot 12-57-06](Screenshot%20from%202025-12-13%2012-57-06.png)

![Screenshot 12-58-36](Screenshot%20from%202025-12-13%2012-58-36.png)

![Screenshot 13-02-54](Screenshot%20from%202025-12-13%2013-02-54.png)

![Screenshot 17-08-58](Screenshot%20from%202025-12-13%2017-08-58.png)

![Screenshot 17-16-53](Screenshot%20from%202025-12-13%2017-16-53.png)

![Screenshot 17-16-58](Screenshot%20from%202025-12-13%2017-16-58.png)

![Screenshot 19-52-06](Screenshot%20from%202025-12-13%2019-52-06.png)

![Screenshot 19-52-14](Screenshot%20from%202025-12-13%2019-52-14.png)

![Screenshot 19-52-21](Screenshot%20from%202025-12-13%2019-52-21.png)

![Screenshot 19-52-26](Screenshot%20from%202025-12-13%2019-52-26.png)

![Screenshot 19-53-20](Screenshot%20from%202025-12-13%2019-53-20.png)

![Screenshot 19-53-30](Screenshot%20from%202025-12-13%2019-53-30.png)


# Day 4: Timing-Aware Physical Design and Custom Cell Integration

## Session Summary
The fourth day of the workshop concentrated on understanding timing behavior before routing, aligning standard cell layouts with routing tracks, and integrating a custom-designed inverter into the OpenLANE flow. The lab sessions emphasized timing optimization, LEF generation, synthesis tuning, and post-synthesis timing verification.


## Pre-Routing Timing Perspective
Before detailed routing begins, it is critical to evaluate timing to prevent late-stage violations. Pre-layout timing analysis allows designers to identify long paths, improper buffering, and clock-related issues early in the flow.

A well-structured clock distribution is necessary to:
- Control clock skew
- Reduce timing uncertainty
- Improve overall design reliability


## Delay-Based Timing Representation

## Concept of Delay Tables
Delay tables capture how much time signals take to propagate through logic cells and interconnects under different conditions. These values form the basis for estimating path delays during synthesis and placement.

## Application in Design Flow
- Used by synthesis tools to calculate arrival and required times
- Helps tools choose optimal cell sizes and buffering
- Assists in meeting timing targets before physical routing


## From Layout Grids to Routing Tracks

## Why Track Alignment Matters
Physical layouts are drawn on grids, while routing follows predefined tracks provided by the technology. Converting grid-based layouts to track-aligned designs ensures compatibility with automated routing.

## Cell Layout Constraints
- Pins should lie on valid routing intersections
- Cell width must align with horizontal track pitch
- Cell height must match vertical track spacing


## Track Configuration Details
Track definitions are provided through the tracks.info file, which includes:
- Track pitch values
- Layer-specific routing directions
- Spacing constraints

This information guides both cell layout and routing feasibility.


## Day 4 Lab Work: Custom Inverter Preparation

## Loading the Inverter Layout
The inverter layout was opened in MAGIC for inspection and modification.


![Screenshot from 2025-12-14 06-42-46](Screenshot%20from%202025-12-14%2006-42-46.png)

![Screenshot from 2025-12-14 06-43-28](Screenshot%20from%202025-12-14%2006-43-28.png)

![Screenshot from 2025-12-14 06-46-17](Screenshot%20from%202025-12-14%2006-46-17.png)

![Screenshot from 2025-12-14 06-52-26](Screenshot%20from%202025-12-14%2006-52-26.png)

![Screenshot from 2025-12-14 13-17-16](Screenshot%20from%202025-12-14%2013-17-16.png)

![Screenshot from 2025-12-14 13-33-02](Screenshot%20from%202025-12-14%2013-33-02.png)

![Screenshot from 2025-12-14 13-33-05](Screenshot%20from%202025-12-14%2013-33-05.png)

![Screenshot from 2025-12-14 13-35-53](Screenshot%20from%202025-12-14%2013-35-53.png)

![Screenshot from 2025-12-14 13-52-48](Screenshot%20from%202025-12-14%2013-52-48.png)

![Screenshot from 2025-12-14 14-00-31](Screenshot%20from%202025-12-14%2014-00-31.png)

![Screenshot from 2025-12-14 14-22-58](Screenshot%20from%202025-12-14%2014-22-58.png)

![Screenshot from 2025-12-14 14-23-39](Screenshot%20from%202025-12-14%2014-23-39.png)

![Screenshot from 2025-12-14 14-28-17](Screenshot%20from%202025-12-14%2014-28-17.png)

![Screenshot from 2025-12-14 14-28-25](Screenshot%20from%202025-12-14%2014-28-25.png)

![Screenshot from 2025-12-14 14-32-37](Screenshot%20from%202025-12-14%2014-32-37.png)

![Screenshot from 2025-12-14 14-32-45](Screenshot%20from%202025-12-14%2014-32-45.png)








