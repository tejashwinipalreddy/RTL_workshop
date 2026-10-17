# VSDBabySoC — SKY130 HD Physical Design and STA

## 1. Project Overview

This repository documents my learning and analysis of the VSDBabySoC physical design flow and multi-corner Static Timing Analysis (STA) using the SKY130 HD standard-cell library.

My existing RTL workshop documentation covers MUX design, BabySoC architecture, synthesis, netlist analysis, and waveform verification. This repository focuses on physical design concepts and timing analysis.

## 2. Physical Design Flow

The RTL-to-GDSII flow includes the following stages:

1. Synthesis
2. Floorplanning
3. Power planning
4. Placement
5. Clock Tree Synthesis (CTS)
6. Routing
7. Parasitic extraction
8. Static Timing Analysis
9. Physical verification
10. GDSII layout generation

These stages describe the overall flow. Only stages supported by the available screenshots or reports should be claimed as completed.

### Physical Design Screenshots

<img width="1919" height="1079" alt="Screenshot 2026-10-10 215236" src="https://github.com/user-attachments/assets/bf8b8862-08d4-41b0-94ea-e0033a9290aa" />

## 3. Multi-Corner Static Timing Analysis

Static Timing Analysis checks whether a design meets its timing constraints without requiring simulation test vectors.

Multi-corner STA evaluates timing under different process, voltage, and temperature (PVT) conditions.

The supplied updated report covers 16 SKY130 HD corners using an 11 ns clock constraint, approximately 90.91 MHz.

### Reported Setup Results

- Total corners analyzed: 16
- Corners with positive setup WNS: 10
- Corners with negative setup WNS: 6
- Best reported setup WNS: +8.22 ns
- Worst reported setup WNS: -31.85 ns

Negative setup slack indicates a timing violation. The supplied results do not demonstrate setup timing closure across all 16 corners.

### STA Report Screenshots
<img width="1338" height="747" alt="image" src="https://github.com/user-attachments/assets/8bc64a12-c768-41c4-bb8d-d4f194ba209d" />

<img width="642" height="450" alt="image" src="https://github.com/user-attachments/assets/6183cc76-6b61-458b-8da9-38db51745fd1" />


## 4. Setup and Hold Timing

**Setup timing:** Checks whether data arrives early enough before the active clock edge.

**Hold timing:** Checks whether data remains stable for the required interval after the active clock edge.

**WNS:** Worst Negative Slack.

**TNS:** Total Negative Slack.

Positive slack indicates that the checked path meets its timing requirement; negative slack indicates a timing violation.

### Timing Analysis Pictures

<img width="604" height="341" alt="image" src="https://github.com/user-attachments/assets/f2bda23e-3d80-48dd-bb35-20603e092040" />

## 5. Results and Observations

The supplied updated STA report shows setup violations in six of the 16 corners. The worst reported setup WNS is -31.85 ns at `ss_n40C_1v28`.

Further investigation would require examining critical timing paths, clock and I/O constraints, cell and net delays, and parasitics.

The updated values have not been independently confirmed by rerunning the complete STA flow. They are documented as supplied results rather than independently verified signoff results.

## 6. Conclusion

This repository presents the physical design concepts and multi-corner timing analysis studied for VSDBabySoC using the SKY130 HD standard-cell library.

The results highlight the importance of evaluating setup timing across multiple PVT corners before claiming timing closure.
