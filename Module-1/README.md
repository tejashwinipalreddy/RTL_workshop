# Module-1 – Verilog RTL Design

# Module-1 – Verilog RTL Design Through Simulation & Yosys Synthesis

## 1. Workshop Overview

This module focuses on the fundamentals of RTL design using Verilog HDL. The complete design flow from writing RTL code to simulation, waveform analysis, synthesis, and gate-level netlist generation is explored through practical exercises.

The objective of this module is to understand how a digital design is described using Verilog and how the RTL code is converted into hardware using synthesis tools.

---

## 2. Digital Design and RTL Design Flow

RTL (Register Transfer Level) design describes the transfer of data between registers and the logical operations performed on that data.

The basic RTL design flow followed in this module is:

1. Design specification
2. RTL coding using Verilog HDL
3. Testbench development
4. RTL simulation
5. Waveform analysis
6. RTL synthesis
7. Technology mapping
8. Gate-level netlist generation

This flow helps in understanding how Verilog code is transformed into a hardware implementation.

---

## 3. Verilog HDL

Verilog Hardware Description Language is used to describe the behavior and structure of digital circuits.

In this module, Verilog is used to implement basic combinational and sequential digital circuits.

Important concepts include:

* Modules and ports
* Input and output declarations
* Continuous assignments
* Procedural blocks
* `always` blocks
* Conditional statements
* Combinational logic
* Testbench creation

---

## 4. Testbench Development

A testbench is used to verify the functionality of a Verilog design.

The testbench provides different input combinations to the Design Under Test (DUT) and observes the corresponding outputs.

The verification process includes:

* Instantiating the DUT
* Applying input stimulus
* Monitoring outputs
* Generating simulation waveforms
* Checking whether the output matches the expected behavior

---

## 5. RTL Simulation using Icarus Verilog

Icarus Verilog is used as the Verilog simulation tool.

The Verilog design and testbench are compiled and simulated to verify the functional behavior of the circuit before synthesis.

Typical simulation flow:

```text
Verilog RTL
     ↓
Testbench
     ↓
Icarus Verilog
     ↓
Simulation
     ↓
Waveform File
```

---

## 6. Waveform Analysis using GTKWave

GTKWave is used to visualize the simulation waveforms generated during RTL simulation.

Waveform analysis helps verify:

* Input transitions
* Output responses
* Timing relationships
* Correct logical behavior
* Errors in RTL implementation

The waveform is compared with the expected truth table or functional behavior of the design.

---

## 7. 2:1 Multiplexer Design

A 2:1 Multiplexer is implemented as a practical RTL design example.

A multiplexer selects one of two input signals based on a select signal.

### Inputs

* `I0` – Input 0
* `I1` – Input 1
* `S` – Select signal

### Output

* `Y` – Selected output

### Functional Operation

When `S = 0`:

```text
Y = I0
```

When `S = 1`:

```text
Y = I1
```

The circuit is implemented using Verilog and verified using a testbench.

---

## 8. RTL Synthesis using Yosys

Yosys is used to synthesize the Verilog RTL design.

Synthesis converts the RTL description into a gate-level representation that can be implemented using standard cells.

The synthesis flow includes:

1. Reading the Verilog RTL
2. Elaborating the design
3. Performing RTL optimization
4. Converting the design into logic gates
5. Mapping the design to available standard cells
6. Generating the gate-level netlist

---

## 9. Technology Mapping and Gate-Level Netlist

Technology mapping converts the synthesized logic into cells available in a particular standard-cell library.

The resulting gate-level netlist represents the actual logic gates/cells used to implement the design.

This step provides an understanding of how RTL constructs are translated into hardware structures.

---

## 10. Practical Exercise

The practical exercise includes:

* Writing Verilog RTL code
* Creating a testbench
* Compiling the design using Icarus Verilog
* Running RTL simulation
* Viewing waveforms using GTKWave
* Synthesizing the design using Yosys
* Observing the synthesized logic
* Generating and analyzing the gate-level netlist

---

## 11. Results

The simulation results and waveform screenshots are included in this module.

The results demonstrate that the RTL design produces the expected outputs for the applied input combinations.

---

## 12. Tools Used

* Verilog HDL
* Icarus Verilog (`iverilog`)
* GTKWave
* Yosys
* Linux
* Git
* GitHub
* SKY130 Standard-Cell Library

---

## 13. Learning Outcomes

After completing this module, I gained practical understanding of:

* RTL design using Verilog
* Writing Verilog testbenches
* RTL simulation
* Waveform analysis
* Digital circuit verification
* RTL synthesis
* Technology mapping
* Gate-level netlist generation
* Basic digital design flow
