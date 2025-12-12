# NASSCOM-VSD-SoC--design-and--Planning-Program
Digital VLSI Soc Design and Planning Training

# VLSI SoC Design and OpenLane Flow – Workshop Documentation

This document summarizes the concepts and practical work completed during the VLSI SoC Design and Planning workshop. The program focused on understanding how a digital hardware description written in RTL is transformed into a manufacturable GDSII layout using the OpenLane open-source ASIC design flow. The workshop covered both theoretical foundations and hands-on implementation steps.

# Overview of RTL to GDSII Flow

The RTL to GDSII flow describes the complete transformation of a behavioral hardware description into its physical layout. This process includes synthesis, floor planning, placement, clock tree construction, routing, and a set of verification checks. The objective of the flow is to ensure that the final layout accurately reflects the intended logical behavior, meets timing requirements, and satisfies foundry design rules.

# Design Abstraction and VLSI Y-Chart

One of the key concepts introduced was the design abstraction in VLSI. The Y-Chart model illustrates how design progresses through three major domains—behavioral, structural, and geometric. This relationship is essential in understanding how high-level descriptions eventually become physical mask layers used for fabrication.

Chip Components: Package and Internal Structure

# A detailed discussion was provided on the internal composition of an integrated circuit. The following components were studied:

1. Core

The region that contains the logic cells, IP blocks, combinational and sequential circuits, and internal routing resources.

2. Die

The physical silicon area containing the core and all peripheral structures. Multiple dies are fabricated on a wafer and later separated.

3. I/O Pads

Interfaces that connect on-chip signals to the external package. These include input, output, bidirectional, and power pads.

4. IP Blocks

Pre-designed modules such as SRAMs, PLLs, ADCs, and DACs. These blocks often require specialized design efforts and are provided by foundries.

5. PDK (Process Design Kit)

A PDK acts as the communication layer between EDA tools and the fabrication process. It includes device models, rule files for DRC/LVS, physical layer definitions, and standard cell libraries.
In this workshop, the SkyWater 130 nm PDK (sky130_fd_sc_hd) was used.

# Introduction to RISC-V

RISC-V is an open standard Instruction Set Architecture (ISA). The ISA defines how software instructions are interpreted by hardware. Key points covered:

Programming languages are converted to machine-level instructions through a software stack involving the OS, compiler, and assembler.

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

Simplified RTL to GDS Flow
1. Synthesis

The RTL description is translated into a gate-level netlist using standard cells. Cells are provided in multiple representations such as Verilog, Liberty (.lib), LEF, SPICE, and GDS.

2. Floor Planning and Power Planning

Defines the layout structure, core area, pin placement, macro positioning, and the power distribution network (power rings, straps, and rails).

3. Placement

Standard cells are arranged in rows.

Global Placement provides an approximate location.

Detailed Placement refines positions to ensure alignment and legality.

4. Clock Tree Synthesis

A balanced clock network is synthesized to minimize skew across all sequential elements. Structures such as H-trees or X-trees may be used.

5. Routing

Global routing generates routing guides, and detailed routing creates the final metal connections according to PDK constraints.

6. Sign-off

The physical design undergoes several final checks:

DRC (Design Rule Check)

LVS (Layout vs Schematic)

Parasitic extraction and STA

Antenna rule checks

Antenna prevention techniques such as bridging and diode insertion were discussed. OpenLane utilizes fake diode insertion initially and replaces them with real diodes if violations are detected after routing.

Detailed OpenLane Flow

The complete OpenLane flow includes:

RTL Synthesis (Yosys, ABC)

Static Timing Analysis

Optional DFT insertion using FAULT

Floorplan and Powerplan generation

Placement and post-placement optimization

CTS

Routing

Physical Verification (Magic, Netgen)

GDSII export

The flow ensures functional correctness and manufacturability at each stage.

Practical Work – Day 1

Hands-on tasks performed include:

Setting up the OpenLane working environment

Running synthesis for sample RTL designs

Inspecting standard cell libraries

Observing floorplan parameters

Opening layout files using Magic

Understanding basic configuration files used in each stage
