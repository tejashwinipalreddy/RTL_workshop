# pd-Module 4: CMOS Inverter Design, Simulation, CMOS Fabrication

## 📌 Project Overview

This project provides a detailed study of CMOS inverter design, starting from transistor-level circuit development and SPICE simulation and continuing through physical layout and CMOS fabrication concepts.

The complete flow covered in this project is:

**CMOS Circuit → SPICE Netlist → DC Analysis → VTC → Switching Threshold → Transistor Sizing → Layout → CMOS Fabrication**

The project shows how a basic CMOS inverter can be studied from both the electrical circuit perspective and the physical semiconductor implementation perspective.

## 🎯 Objectives

The main objectives of this project are:

1. To understand the working of a CMOS inverter using NMOS and PMOS transistors.
2. To design and analyse a transistor-level CMOS inverter using SPICE.
3. To examine how transistor dimensions affect inverter performance.
4. To generate and study the Voltage Transfer Characteristic (VTC).
5. To identify the switching threshold voltage, \(V_M\).
6. To analyse CMOS inverter stability and transistor sizing.
7. To study the effect of \(W/L\) ratios on inverter operation.
8. To understand the physical implementation of a CMOS standard cell.
9. To study the important steps of the 16-mask CMOS fabrication process.
10. To understand active-region formation, well formation, gate formation, LDD implantation, source/drain formation, contacts, and metallization.
11. To relate circuit-level simulation with semiconductor manufacturing technology.

## 🔧 Tools & Technologies

| Tool / Technology   | Purpose                           |
| ------------------- | --------------------------------- |
| **SPICE / NGSPICE** | Electrical circuit simulation     |
| **CMOS Inverter**   | Circuit being analysed            |
| **NMOS & PMOS**     | MOS transistor devices            |
| **VTC Analysis**    | Static inverter analysis          |
| **Magic VLSI**      | Physical layout design            |
| **SKY130A PDK**     | CMOS technology and design rules  |
| **Linux Terminal**  | Simulation and design operations  |
| **Git & GitHub**    | Version control and documentation |

## 🔬 Project Workflow

**CMOS Inverter Design**
↓
**SPICE Netlist Creation**
↓
**DC Simulation**
↓
**VTC Generation**
↓
**Switching Threshold Analysis**
↓
**Transistor Sizing**
↓
**Physical Layout**
↓
**16-Mask CMOS Fabrication Study**
↓
**Contacts & Local Interconnect**
↓
**Higher-Level Metal**
↓
**Complete CMOS Structure**

---

## 1. SPICE CMOS Inverter Design
<img width="947" height="435" alt="image" src="https://github.com/user-attachments/assets/db47a983-04b1-4d38-b386-847b5d0bd8a7" />


The first step is to create the transistor-level CMOS inverter using a SPICE netlist.

The circuit contains:

* One PMOS transistor connected to \(V_{DD}\)
* One NMOS transistor connected to \(V_{SS}\)
* A common gate input
* A common drain output
* A capacitive load connected at the output
* Specified transistor dimensions
* CMOS device model definitions
* DC simulation instructions

The initial transistor dimensions are:

$$
W_n = W_p = 0.375\mu m
$$

$$
L_n = L_p = 0.25\mu m
$$

Therefore,

$$
W_n/L_n = W_p/L_p = 1.5
$$

The SPICE deck builds the inverter by defining the transistors, their dimensions, supply voltages, input conditions, output load, device models, and simulation environment required for electrical analysis.

---

## 2. SPICE Simulation — Initial Device Configuration
<img width="950" height="435" alt="image" src="https://github.com/user-attachments/assets/5a768a2c-e643-47fd-b3dd-cd4695bd106c" />


This stage represents the CMOS inverter simulation with identical NMOS and PMOS sizing.

The simulation establishes the basic relationship between:

**Input Voltage → Transistor State → Output Voltage**

The inverter works through complementary operation of the two transistors:

* When the input is LOW, the PMOS turns ON and the NMOS remains OFF.
* When the input is HIGH, the NMOS turns ON and the PMOS turns OFF.
* During the transition region, both transistors affect the output.

SPICE translates the transistor-level circuit description into measurable electrical results, making it possible to study inverter behaviour before creating the physical layout.

---

## 3. CMOS Inverter Voltage Transfer Characteristic
<img width="850" height="732" alt="image" src="https://github.com/user-attachments/assets/07fba9f5-1105-4feb-a957-999a6bc93c49" />


The simulation generates the Voltage Transfer Characteristic (VTC) of the CMOS inverter.

The VTC represents the relationship:

$$
V_{out} = f(V_{in})
$$

The curve can be divided into three major regions:

**HIGH OUTPUT → TRANSITION → LOW OUTPUT**

$$
V_{out} \approx V_{DD}
$$

during the HIGH-output region, while:

$$
V_{out} \approx 0
$$

during the LOW-output region.

The steep transition indicates that the CMOS inverter provides high voltage gain around its switching region.

A small variation in input voltage can therefore produce a significant change in output voltage, which is an important characteristic of CMOS logic.

---

## 4. Transistor Sizing Comparison
<img width="844" height="392" alt="image" src="https://github.com/user-attachments/assets/df819e8b-88a1-40c0-b3f7-c7a4c52557a4" />


Two different CMOS inverter configurations are analysed.

### Configuration 1

$$
W_n = W_p = 0.375\mu m
$$

$$
W_n/L_n = W_p/L_p = 1.5
$$

### Configuration 2

$$
W_n = 0.375\mu m
$$

$$
W_p = 0.9375\mu m
$$

$$
W_n/L_n = 1.5
$$

$$
W_p/L_p = 3.75
$$

Increasing the PMOS width changes the relative driving capability of the PMOS and NMOS devices. As a result, the switching characteristics of the inverter also change.

This comparison demonstrates that transistor dimensions are not only physical design parameters; they also have a direct effect on the electrical behaviour of the inverter.

---

## 5. CMOS Inverter Robustness — Switching Threshold
<img width="847" height="393" alt="image" src="https://github.com/user-attachments/assets/c12fb8f9-5119-496a-934f-09f8263d1b47" />


The switching threshold voltage \(V_M\) is defined as the point where:

$$
V_{in} = V_{out}
$$

The results compare inverter characteristics for different transistor sizing conditions and show how the switching point changes according to transistor strength.

The observed values include approximately:

$$
V_M \approx 0.98V
$$

and

$$
V_M \approx 1.2V
$$

depending on the transistor sizing configuration.

The switching threshold represents the balance point at which the inverter changes between its two logic states. Transistor sizing determines the position of this balance point.

---

## 6. Mathematical Analysis of Switching Threshold
<img width="844" height="392" alt="image" src="https://github.com/user-attachments/assets/6de08395-f0f0-4c12-80a7-79c1fef8ffdd" />


The switching threshold can also be determined analytically by considering the relative strengths of the NMOS and PMOS transistors.

The analysis considers parameters such as:

1. \(W_n/L_n\)
2. \(W_p/L_p\)
3. \(K_n\)
4. \(K_p\)
5. Saturation voltage
6. Device threshold parameters

Therefore, the switching threshold obtained from simulation is not an arbitrary value. It depends strongly on the relative drive strengths of the two transistors.

The mathematical analysis helps explain why changing transistor dimensions causes the switching threshold observed in the SPICE VTC to shift.

---

## 7. Final Switching-Threshold Comparison
<img width="841" height="395" alt="image" src="https://github.com/user-attachments/assets/f6bda6d0-1a8f-4d52-93b9-9413b913ee8d" />


The final characterization combines the inverter VTC with transistor sizing information and timing-related results.

It demonstrates the effect of changing transistor dimensions on the electrical response of the CMOS inverter.

An important CMOS design principle highlighted here is:

**The relative strength of NMOS and PMOS devices determines the switching behaviour and influences timing characteristics.**

The characterization process therefore connects the simulation results with transistor dimensions and device-strength relationships, making the SPICE plots easier to interpret.

---

## 8. Physical CMOS Layout
<img width="856" height="422" alt="image" src="https://github.com/user-attachments/assets/069c3549-056a-4621-ad35-d0e6e4b0db6d" />


After completing the electrical analysis, the project proceeds to the physical implementation of the CMOS inverter.

The layout shows how the CMOS inverter is physically arranged using technology-specific layers.

The major physical components include:

1. PMOS region
2. NMOS region
3. Polysilicon gate
4. Active regions
5. Contacts
6. Metal interconnections
7. Power and ground connections

This step converts the transistor schematic into a geometric representation that can be used as the basis for fabrication.

The schematic describes the electrical operation of the circuit, while the layout defines how the circuit is physically constructed on silicon.

---

# 9. 🏭 CMOS FABRICATION PROCESS

The following stages describe the 16-mask CMOS fabrication sequence covered in the project.

## 9.1 Active Region Formation
<img width="846" height="387" alt="image" src="https://github.com/user-attachments/assets/4fced85c-b35a-4b5b-91b3-0ea933142d3f" />


The fabrication process begins by defining the areas in which the transistors will be created.

The figure contains:

* P-type silicon substrate
* Silicon nitride masking
* Photoresist
* Field oxide
* LOCOS isolation
* Bird's-beak effect

The process used is **LOCOS — Local Oxidation of Silicon**.

Field oxide is used to isolate the active transistor regions from the surrounding silicon.

Before the transistor can be constructed, the silicon surface must first be separated into active and isolation areas. LOCOS provides the necessary isolation for controlled transistor fabrication.

---

## 9.2 N-Well and P-Well Formation
<img width="860" height="389" alt="image" src="https://github.com/user-attachments/assets/7b3f3ca5-0564-4ae0-a820-9659586726eb" />


CMOS technology requires regions with different conductivity types so that both NMOS and PMOS devices can be fabricated.

Ion implantation is used during this stage to create the required well regions.

The implantation process establishes the appropriate electrical regions within the silicon substrate for the formation of NMOS and PMOS transistors.

Well formation therefore provides the correct body environment in which the complementary transistors can operate.

---

## 9.3 Threshold Voltage & Body Effect
<img width="849" height="375" alt="image" src="https://github.com/user-attachments/assets/c2896ac6-127e-4dd3-87da-ca996b8633ef" />


This stage explains the theoretical basis of MOS threshold voltage and body effect.

The threshold voltage depends on several parameters, including:

* **\(V_{T0}\)** — threshold voltage at zero body bias
* **\(\gamma\)** — body-effect coefficient
* **\(V_{SB}\)** — source-to-body voltage
* **\(\Phi_F\)** — Fermi potential
* **\(N_A\)** — doping concentration
* **\(C_{ox}\)** — oxide capacitance

MOS transistor behaviour is therefore affected not only by the gate voltage but also by substrate doping, oxide characteristics, and body bias.

---

## 9.4 Gate Formation — Initial Stage
<img width="843" height="392" alt="image" src="https://github.com/user-attachments/assets/6333138b-1ccb-49e4-a056-b373b4bba56d" />


The next fabrication step is the formation of the transistor gate structure.

The gate serves as the control terminal of a MOS transistor and determines whether a conductive channel can form between the source and drain.

The figure illustrates the processing steps involved in gate formation and implantation.

Gate fabrication creates the main control structure that determines how current flows through the transistor channel.

---

## 9.5 Gate Formation — Completed Structure
<img width="832" height="388" alt="image" src="https://github.com/user-attachments/assets/7371f22f-6012-4d88-a333-faba56be6c68" />


This stage continues the gate-formation process and shows the resulting structure after the required processing steps.

The gate separates the source and drain regions and controls the channel located underneath it.

At this stage, the physical structure needed to control current through the transistor channel becomes clearly defined.

---

## 9.6 LDD Formation — Initial Implantation
<img width="838" height="377" alt="image" src="https://github.com/user-attachments/assets/10f487f8-d0b8-4492-bcd7-22b75ce9adcf" />


The Lightly Doped Drain (LDD) process creates lightly doped regions close to the source and drain areas.

LDD structures help control electric-field conditions near the drain and contribute to improved device reliability.

The controlled doping introduced during this stage helps manage high electric fields inside the transistor.

---

## 9.7 LDD Formation — Phosphorus Implantation
<img width="860" height="393" alt="image" src="https://github.com/user-attachments/assets/c046de9a-84f0-4627-b46d-835caadd1174" />


This stage shows the phosphorus implantation used as part of the LDD formation process.

Ion implantation is carefully controlled to modify the conductivity of selected regions of the silicon.

Through controlled doping, the required conductivity profile for transistor operation is established.

---

## 9.8 Side-Wall Spacer Formation

After LDD implantation, side-wall spacers are created around the transistor gate.

These spacers provide the required physical separation between the gate and the heavily doped source/drain regions formed later.

The spacer therefore controls the distance between the gate and the heavily doped regions, providing accurate transistor geometry.
<img width="852" height="388" alt="image" src="https://github.com/user-attachments/assets/4fab79ac-0e10-40ba-bffa-6c6513f87041" />


---

## 9.9 Source & Drain Formation
<img width="846" height="395" alt="image" src="https://github.com/user-attachments/assets/1411bede-ceac-4dc4-ba8a-05e754c2144f" />


The next fabrication stage creates the final source and drain regions using high-temperature processing.

At this point, the main transistor structure consists of:

```text
        Gate
         │
    ┌────┴────┐
    │ Channel │
────┴─────────┴────
 Source       Drain
```

The source and drain provide the terminals through which current enters and leaves the transistor.

With these regions formed, the basic semiconductor structure required for controlled current flow is complete.

---

## 9.10 Contacts & Local Interconnect — Titanium Deposition
<img width="811" height="370" alt="image" src="https://github.com/user-attachments/assets/5880e857-420c-4410-861f-6244b4499439" />


After the transistor structures have been completed, electrical connections need to be established.

This stage shows titanium deposition on the wafer surface using sputtering.

The deposited material prepares the structure for forming low-resistance electrical connections between the semiconductor regions and the interconnect system.

Contact technology provides the required connection between the fabricated transistor terminals and the wiring structure.

---

## 9.11 Contact Formation
<img width="770" height="363" alt="image" src="https://github.com/user-attachments/assets/8304ceea-f087-41ee-851b-bebbd251b374" />


The next stage defines the contact regions used to connect:

**Source**
**Drain**
**Gate**

to the local interconnect structure.

These contacts create conductive paths between the transistor and the higher-level wiring.

Contacts therefore transform individual transistor structures into electrically accessible devices that can be connected into larger circuits.

---

## 9.12 Higher-Level Metal Formation
<img width="717" height="366" alt="image" src="https://github.com/user-attachments/assets/80eb3292-10f0-4467-9735-c5c139a889cf" />


CMOS fabrication continues by creating additional levels of metal interconnect.

These metal layers provide pathways for:

* Signals
* Power
* Ground

to travel throughout the chip.

The interconnect hierarchy connects the individually fabricated devices into a complete circuit network.

Higher-level metal can be considered the main routing network that carries electrical connections across different areas of the integrated circuit.

---

## 9.13 Complete CMOS Structure
<img width="692" height="365" alt="image" src="https://github.com/user-attachments/assets/92edee5b-f9b4-4438-a85a-b949147f5c7d" />


The final fabrication image shows the completed CMOS structure.

The structure contains:

* Silicon substrate
* Well regions
* Active regions
* Gate structures
* Source/drain regions
* Contacts
* Local interconnect
* Higher-level metal

Together, these layers form a multilayer CMOS structure where semiconductor devices and metal interconnections operate as one integrated system.

The fabrication sequence demonstrates how individual layers are gradually constructed to transform bare silicon into functional CMOS hardware.

---

# 10. Layout and Abstract View

The conversion from a circuit diagram into a physical silicon implementation begins with the standard-cell layout.

Using the **SKY130A technology**, the CMOS inverter is represented as a physical arrangement of different layers corresponding to its implementation on silicon.

The important layers include:

* **Metal layers** – used for electrical routing.
* **Polysilicon** – forms the transistor gate structures.
* **Diffusion** – represents the active source and drain regions.
* **Contacts** – connect different physical layers.
* **Well regions** – provide the required transistor body structures.
* **Power and ground rails** – distribute VDD and GND across the cell.

Along with the detailed layout, an abstract view provides a simplified representation of the standard cell containing the physical information required by the digital design flow.

The detailed layout and abstract view together define the physical identity of the standard cell. Both are checked to ensure that the geometry, layer arrangement, and connectivity are correctly implemented.
<img width="656" height="376" alt="image" src="https://github.com/user-attachments/assets/165d3ea3-2810-42e0-ac51-52279bf03f6f" />


**Figure: Layout and abstract representation of the standard cell**

This stage gives the circuit its first physical representation, converting transistor-level logic into an organized silicon structure.

---

# 11. Defining the Cell Boundary

A standard cell requires a clearly specified physical area.

After the layout is completed, a cell boundary is created to identify the exact region occupied by the circuit.

The boundary specifies the width and height of the cell and provides a fixed area in which the transistor and routing structures are arranged.

A correctly defined boundary supports:

* Consistent cell dimensions
* Accurate placement
* Alignment with neighbouring cells
* Correct VDD and GND rail locations
* Compatibility with the standard-cell library

This organized structure allows multiple cells to be placed next to one another or stacked within a larger physical-design system while maintaining proper alignment.
<img width="668" height="421" alt="image" src="https://github.com/user-attachments/assets/ba2b1386-4ae6-4640-98f7-277591954dfe" />


**Figure: Defined standard-cell boundary**

The cell boundary acts as the physical framework of the standard cell and allows it to integrate smoothly with other cells in a larger digital design.

---

# 12. Power and Ground Connectivity

After defining the cell structure, the power and ground connections are established.

The CMOS cell uses two primary supply connections:

* **VDD** – provides the positive operating supply.
* **GND** – provides the ground or reference potential.

These supply rails are connected to the appropriate transistor regions through the required physical layers.

In a CMOS inverter, the PMOS network is connected towards the supply side, while the NMOS network is connected towards ground.

Proper routing is necessary to ensure that both complementary transistor networks operate correctly.

Reliable power connectivity is important for circuit operation as well as for maintaining the physical structure expected by the standard-cell library.
<img width="672" height="430" alt="image" src="https://github.com/user-attachments/assets/6c60fdab-94eb-451e-9291-bf6ebf3f1320" />


**Figure: Power and ground connections in the layout**

Power provides the energy required by the cell, while ground completes the electrical return path required for CMOS operation.

---

# 13. Layout Extraction

A physical layout contains geometric information, whereas circuit simulation requires an electrical representation.

Layout extraction provides the connection between these two forms.

After completing the layout, the extraction process interprets the physical shapes and converts them into an electrical circuit representation.

The extraction identifies:

* Transistors present in the layout
* Electrical nodes
* Connections between devices
* Physical device dimensions
* Power and ground paths
* Parasitic elements

This information is used to generate a SPICE-compatible representation of the implemented circuit.

Unlike an ideal schematic, the extracted representation contains information obtained from the actual physical layout. This makes it useful for analysing the circuit after physical implementation.
<img width="816" height="784" alt="image" src="https://github.com/user-attachments/assets/bb68284e-2d58-4ec5-bd11-45a8201ef07a" />


**Figure: Extraction of the layout**

Layout extraction converts physical geometry into electrical information that can be used for circuit simulation.

---

# 14. Generating the Extracted Netlist

After extraction, the generated files are checked to verify that the physical layout has been correctly converted into electrical connectivity.

The extracted netlist provides a text-based description of the devices and connections identified in the layout.

It contains the information required to reproduce the physical implementation for additional simulation and analysis.

The generated files are inspected before continuing to ensure that the required device and connectivity information is available.

Typical outputs include extracted layout information and SPICE-compatible netlist data.
<img width="818" height="795" alt="image" src="https://github.com/user-attachments/assets/8f8d569b-b58a-4a23-895c-19a051f78f26" />


**Figure: Generated extracted files and netlist**

The netlist acts as an electrical representation recovered from the physical layout, describing how the devices and connections are linked together.

---

# 15. Creating the SPICE File

The extracted information is then converted into a SPICE file that can be used for simulation.

The file combines the device models, circuit connections, and simulation parameters required by NGSPICE.

The SPICE representation contains:

* Technology and device model information
* Standard-cell subcircuit definition
* Input and output nodes
* VDD connection
* GND connection
* Extracted transistor information
* Simulation parameters

The extracted transistor and connectivity information is organized into an appropriate subcircuit so that the physically implemented CMOS inverter can be analysed electrically.
<img width="606" height="586" alt="image" src="https://github.com/user-attachments/assets/b9aad0c7-8e25-434d-bb2c-3f849da30a07" />


**Figure: SPICE file generated for simulation**

At this stage, the extracted layout receives a simulation-ready representation that allows its physical implementation to be evaluated using SPICE.

---

# 16. Transient Simulation using NGSPICE

Once the SPICE representation is prepared, the extracted CMOS inverter is simulated using NGSPICE.

Transient analysis applies a time-varying input signal and observes the corresponding output response over time.

The circuit is operated using the required supply conditions while monitoring:

* **Input**
* **Output**
* **VDD**
* **GND**

The changing input signal allows the simulator to observe the switching behaviour of the inverter and verify that the extracted circuit is correctly connected and capable of successful simulation.

This stage connects the physical implementation back to measurable electrical behaviour.
<img width="607" height="587" alt="image" src="https://github.com/user-attachments/assets/b74bee9e-c723-4040-b64a-293c6d605931" />


**Figure: NGSPICE transient analysis**

The extracted circuit is tested under switching conditions to verify whether the physical CMOS implementation behaves as the intended inverter.

---

# 17. Input and Output Waveforms

The transient simulation produces input and output waveforms that provide direct evidence of inverter operation.

When the input alternates between LOW and HIGH, the output changes in the opposite manner, as expected from a CMOS inverter.

| Input | Output |
| ----- | ------ |
| LOW   | HIGH   |
| HIGH  | LOW    |

The fundamental inverter relationship is:

**Output = NOT(Input)**

The output also approaches the expected supply and ground levels, confirming that the extracted standard cell continues to provide the required digital logic behaviour.
<img width="607" height="578" alt="image" src="https://github.com/user-attachments/assets/275f1a5e-bde5-4f21-8d87-f85c24f0745b" />


**Figure: Simulated input and output transient waveforms**

The waveforms demonstrate the complementary relationship between the input and output during inverter switching.

---

# 18. Physical Verification and Layout Analysis

After creating the layout, the physical structure is inspected to ensure that the CMOS cell has been implemented correctly.

Important layers and connections are checked, including:

* Transistor regions
* Diffusion
* Polysilicon
* Contacts
* Metal interconnections
* Power network

The verification process checks for:

* Correct electrical connectivity
* Proper VDD and GND distribution
* Correct cell boundary
* Appropriate technology-layer usage
* Correct PMOS and NMOS arrangement

A successful physical layout must not only appear correct geometrically but should also preserve the electrical function of the original circuit.

---

# 19. Standard Cell Layout Structure

The CMOS inverter follows the conventional organization used in standard-cell design.

The PMOS network is placed in the upper portion of the cell and is connected towards the VDD rail. The NMOS network is placed below it and connects towards GND.

The input signal is connected to the transistor gates, while the output is obtained from the shared node between the pull-up and pull-down networks.

This arrangement produces a compact and repeatable cell structure that can be integrated with other standard cells in a larger digital design.

The physical organization directly reflects the logic structure:

**PMOS pulls the output upward, NMOS pulls it downward, and the shared node forms the inverter output.**

---

# 20. Extraction and Parasitic Information

A completed layout contains more than ideal transistor connections. Physical dimensions and interconnect geometry introduce additional electrical effects.

During layout extraction, the physical shapes are converted into an electrical representation so that these real-world effects can be considered during simulation.

Extracted parasitic information can influence:

* Propagation delay
* Rise time
* Fall time
* Output transition speed
* Dynamic switching behaviour

Therefore, post-layout simulation provides a more realistic view of circuit performance compared with an ideal schematic-level simulation.

---

# 21. SPICE Model and Device Parameters

The extracted CMOS inverter uses the device and technology information provided by the **SKY130A PDK**.

The transistor models provide NGSPICE with the electrical parameters necessary to reproduce MOS-device behaviour during simulation.

Combining the extracted layout information with technology-specific device models creates a simulation that more closely represents the physically implemented circuit instead of an ideal transistor-level model.

This gives a better basis for evaluating the standard cell before it is integrated into a larger digital system.

---

# 22. Simulation Setup

Before transient simulation is performed, the extracted circuit must be supplied with the required operating conditions and input signal.

The supply is connected between VDD and GND, while the inverter input receives a time-dependent digital waveform.

The simulator then observes the response of the output node.

The main characteristics examined are:

* Correct logical operation
* Expected HIGH and LOW voltage levels
* Output switching transitions
* Timing characteristics
* Stable circuit response

These conditions provide the basis for analysing the behaviour of the extracted CMOS inverter.

---

# 23. CMOS Inverter Operation

The CMOS inverter performs its logic function through the complementary switching behaviour of PMOS and NMOS transistors.

### Input LOW

When the input is LOW:

* The PMOS transistor turns ON.
* The NMOS transistor turns OFF.
* The output is pulled towards VDD.
* The output becomes HIGH.

### Input HIGH

When the input changes to HIGH:

* The PMOS transistor turns OFF.
* The NMOS transistor turns ON.
* The output is pulled towards GND.
* The output becomes LOW.

Thus, the CMOS inverter always produces the logical complement of its input.

---

# 24. Rise and Fall Behaviour

The output of a CMOS inverter does not change between logic states instantaneously.

When the input changes from LOW to HIGH, the output changes from HIGH to LOW.

Similarly, when the input changes from HIGH to LOW, the output moves from LOW to HIGH.

The slope observed during these transitions is affected by the charging and discharging of capacitances within the circuit.

Physical interconnections and extracted parasitic components can further influence these transition characteristics.

Therefore, the rising and falling sections of the waveform provide useful information about the dynamic performance of the implemented cell.

---

# 25. Timing Behaviour

Transient simulation provides information about how quickly the standard cell responds to changes at its input.

Important timing parameters include:

* Rise time
* Fall time
* Propagation delay
* Input transition time
* Output transition time

These parameters become increasingly important when multiple standard cells are connected to build larger digital systems.

Transistor dimensions, physical layout, load conditions, and parasitic effects can all affect the final timing performance.

Functionality explains **what the cell does**, while timing explains **how quickly it performs the operation**.

---

# 26. Voltage Levels

The simulated waveform is also checked to determine whether the inverter reaches the expected logic voltage levels.

For correct CMOS operation:

* The HIGH output should approach the supply voltage.
* The LOW output should approach the ground potential.

These voltage levels indicate whether the pull-up and pull-down networks are functioning correctly.

Therefore, the waveform provides information about both the logical operation and electrical performance of the inverter.

---

# 27. Transistor Sizing and Performance

The physical dimensions of the PMOS and NMOS transistors significantly influence CMOS inverter performance.

Changing transistor dimensions can affect:

* Drive strength
* Rise time
* Fall time
* Propagation delay
* Power consumption
* Switching characteristics

Proper sizing is therefore required to obtain a balanced response between the pull-up and pull-down networks.

The dimensions selected during layout are also reflected in the extracted circuit, allowing their effect to be observed during post-layout simulation.

---

# 28. Layout-to-Simulation Correlation

One of the main goals of this project is to establish a connection between the physical implementation and the electrical behaviour of the circuit.

The complete process can be represented as:

**Layout → Extraction → SPICE Netlist → NGSPICE Simulation → Waveform Analysis**

The layout defines the physical geometry of the standard cell.

The extraction stage interprets this geometry and generates the corresponding electrical representation.

The resulting SPICE netlist is prepared for NGSPICE simulation, where the circuit is tested using the required input and supply conditions.

Finally, the simulated waveforms are analysed to determine whether the physical inverter behaves according to its intended logic.

---

# 🔑 Key Learnings

* Understood the **working principle of a CMOS inverter** using complementary PMOS and NMOS transistors.

* Learned how to perform **SPICE and NGSPICE simulations** to study CMOS circuit behaviour.

* Analysed **input/output waveforms, voltage levels, rise time, fall time, and propagation delay**.

* Studied how **transistor sizing** influences drive strength, switching speed, delay, and power consumption.

* Learned how to transform a circuit schematic into a **physical CMOS standard-cell layout**.

* Understood the importance of **cell boundaries, layer arrangement, and standard-cell organization**.

* Implemented and verified **VDD and GND connections** in the physical layout.

* Learned how **layout extraction** converts physical geometry into an electrical netlist.

* Understood the effect of **parasitic elements** on post-layout circuit behaviour.

* Gained practical experience with the **SKY130A PDK, Magic VLSI, SPICE, and NGSPICE** flow.

* Learned how to relate **physical layout results with simulated electrical behaviour**.

* Studied the **16-mask CMOS fabrication process**, covering well formation through metallization.

* Understood the overall semiconductor design flow:

**Circuit Design → Simulation → Characterization → Layout → Extraction → Post-Layout Simulation → Fabrication**

* 🚀 Developed a broader understanding of how a **transistor-level circuit is converted into a verified physical silicon implementation**.

---

# 🎯 Conclusion

This project provided an end-to-end understanding of the **CMOS inverter**, beginning with transistor-level operation and continuing through physical layout, post-layout simulation, and CMOS fabrication.

The inverter was initially analysed as an electrical circuit to understand its logic operation and electrical characteristics. SPICE and NGSPICE simulations were used to examine input-output relationships, switching behaviour, voltage levels, timing characteristics, and the influence of transistor sizing.

The circuit was then implemented as a **SKY130A standard-cell layout**, converting the abstract transistor circuit into a physical structure consisting of diffusion, polysilicon, contacts, metal interconnects, wells, and power rails.

The completed layout was extracted into an electrical representation, establishing the important relationship between **the physical structure on silicon** and **the behaviour observed during simulation**. Post-layout analysis also demonstrated how physical effects and parasitic elements can influence circuit performance.

Finally, the study of the **16-mask CMOS fabrication process** completed the overall journey by showing how carefully designed semiconductor structures can be converted into physical devices on a silicon wafer.
