# Module-0 – RTL Design and Synthesis Workshop

## 1. Introduction

This module provides an introduction to RTL (Register Transfer Level) design and the complete RTL-to-synthesis flow.

The objective is to understand how a digital circuit is described using Hardware Description Languages (HDLs), simulated to verify its functionality, and synthesized into a gate-level representation.

This module also introduces the Linux environment, Git, GitHub, and the basic tools required for RTL design and synthesis.

---

## 2. What is RTL Design?

RTL stands for Register Transfer Level.

RTL design describes:

- How data is transferred between registers
- What operations are performed on the data
- How control signals affect the operation of a digital circuit

RTL is an important stage in the digital IC design process because it provides a hardware-oriented description of the intended functionality.

Verilog HDL is commonly used to describe RTL designs.

---

## 3. RTL Design Flow

The basic RTL design flow consists of several stages:

```text
Design Specification
        ↓
RTL Coding
        ↓
Functional Simulation
        ↓
RTL Verification
        ↓
Logic Synthesis
        ↓
Technology Mapping
        ↓
Gate-Level Netlist
