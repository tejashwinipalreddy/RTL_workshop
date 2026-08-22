# 📁 RTL Workshop — Module 04
### Case File: Blocking Assignments and the Simulation–Synthesis Divide

---

`STATUS: CLOSED`  `TOOLS: Icarus Verilog · GTKWave · Yosys · SKY130`  `CASES LOGGED: 8`

---

## 🔎 Opening Statement

Every Verilog engineer eventually runs into a design that simulates perfectly and synthesizes into something else entirely. This module is a record of that phenomenon, caught in the act. Two multiplexers — one clean, one flawed — and a standalone look at blocking-assignment execution order are used to pin down exactly *why* a simulator and a synthesis tool can disagree about the same fifteen lines of code.

The short version: **`=` (blocking) executes immediately and in program order.** That property is a feature in combinational logic and a liability everywhere else. This case file traces that property from first principles to a real, reproducible mismatch.

---

## ⚗️ Method

| Step | Action |
|---|---|
| 1 | Write RTL |
| 2 | Compile & simulate with `iverilog` |
| 3 | Inspect waveform in `GTKWave` |
| 4 | Synthesize with `Yosys`, mapped to `SKY130` |
| 5 | Compare synthesized schematic against simulated behavior |

---

## 📂 Case Index

| Case No. | Filed Under | Verdict |
|---|---|---|
| 04-A | 2:1 MUX — RTL Simulation | Clean |
| 04-B | 2:1 MUX — Synthesis & Cell Mapping | Clean |
| 04-C | 2:1 MUX — Extended Verification | Clean |
| 04-D | Faulty MUX — First Symptom | Anomaly Detected |
| 04-E | Faulty MUX — Root Cause Confirmed | Latch Inferred |
| 04-F | Blocking Assignment — Execution Order | Behavioral Note |
| 04-G | Blocking Assignment — Synthesized Result | Clean |
| 04-H | Blocking Assignment — Ordering Dependency | Behavioral Note |

---

## Case 04-A · 2:1 MUX — RTL Simulation

**Filed as:** Baseline reference

A 2:1 multiplexer coded with a ternary expression was compiled and simulated to establish what a correctly-behaving combinational output should look like.

```bash
iverilog -o mux ternary_operator_mux.v tb_ternary_operator_mux.v
gtkwave ternary_operator_mux.vcd
```

**Evidence:** Output tracks the selected input continuously; no gaps, no held states.

<img width="2119" height="1077" alt="image" src="https://github.com/user-attachments/assets/90364488-072f-4fcd-bb61-00ae2bfc3b3d" />

**Verdict:** Clean. Filed as the module's reference behavior.

---

## Case 04-B · 2:1 MUX — Synthesis & Cell Mapping

**Filed as:** Hardware confirmation of Case 04-A

The same RTL was pushed through Yosys and mapped onto the SKY130 standard-cell library.

```bash
yosys
read_verilog mux_generate.v
synth -top mux_generate
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

<img width="2124" height="1175" alt="image" src="https://github.com/user-attachments/assets/e81fdc83-df72-475e-824b-638252691514" />



**Evidence:** The ternary expression collapsed cleanly into a single SKY130 MUX cell — no residual gates, no latch.
**Verdict:** Clean. Simulation and synthesis agree exactly.

---

## Case 04-C · 2:1 MUX — Extended Verification

**Filed as:** Stress test of the baseline

A broader input sweep was applied to rule out the possibility that Case 04-A only looked clean because of a narrow test vector.

```bash
iverilog -o mux mux_generate.v tb_mux_generate.v
gtkwave mux_generate.vcd
```


**Evidence:** Correct output for every select/input combination tested.

**Verdict:** Clean. Baseline holds under expanded scrutiny.

---

## Case 04-D · Faulty MUX — First Symptom

**Filed as:** Anomaly report

A second multiplexer, written with incomplete assignments inside its `always` block, was tested under the same conditions as Case 04-A.

```bash
iverilog -o bad_mux bad_mux.v tb_bad_mux.v
gtkwave bad_mux.vcd
```

<img width="2125" height="1196" alt="image" src="https://github.com/user-attachments/assets/6e63e509-d22a-4315-861f-10a7efbece28" />



**Evidence:** Output behavior deviates from the Case 04-A reference — it no longer tracks the select line cleanly.
**Verdict:** Anomaly logged. Flagged for root-cause investigation in Case 04-E.

---

## Case 04-E · Faulty MUX — Root Cause Confirmed

**Filed as:** Closure of Case 04-D

The anomaly from Case 04-D was re-examined, isolating exactly when the output fails to update.

```bash
iverilog -o bad_mux bad_mux.v tb_bad_mux.v
gtkwave bad_mux.vcd
```


**Evidence:** The output freezes at its prior value precisely where the RTL skips an assignment — the signature of latch inference.
**Root cause:** Incomplete conditional coverage in the `always` block forces the synthesis tool to insert a latch to preserve the unassigned state. The resulting hardware does **not** match a naive reading of the RTL.
**Verdict:** Case closed. This is synthesis-simulation mismatch, caught pre-silicon.

---

## Case 04-F · Blocking Assignment — Execution Order

**Filed as:** Mechanism investigation

Set aside the MUX examples entirely and isolate blocking-assignment behavior on its own.

```bash

iverilog -o blocking blocking_caveat.v tb_blocking_caveat.v<img width="2090" height="1190" alt="image" src="https://github.com/user-attachments/assets/e4c7d41a-8d37-4c2a-8f96-516a4f563ac1" />

gtkwave blocking_caveat.vcd
```

<img width="2090" height="1190" alt="image" src="https://github.com/user-attachments/assets/b62ae3bc-4020-4a20-954d-cb36177a5da2" />

**Evidence:** Each `=` statement takes effect the instant it executes; the very next line in the block already sees the new value.
**Finding:** This immediate, program-order execution is exactly why blocking assignments suit **combinational** logic — and why they're risky in **sequential** logic, where designers usually expect the *old* register value on the right-hand side, not one just written moments earlier in the same block.

---

## Case 04-G · Blocking Assignment — Synthesized Result

**Filed as:** Hardware confirmation of Case 04-F

The same blocking-assignment design was synthesized to check whether Yosys's interpretation matches the simulated intent.

```bash
yosys
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```


**Evidence:** The SKY130-mapped netlist matches the behavior observed in Case 04-F.
**Verdict:** Clean. For this design, disciplined combinational use of blocking assignments synthesizes exactly as expected.

---

## Case 04-H · Blocking Assignment — Ordering Dependency

**Filed as:** Final mechanism proof

One more pass through the blocking-assignment waveform, tracing a signal that consumes a value written earlier in the *same* procedural block, on the *same* simulation time step.

```bash
iverilog -o blocking_past blocking_caveat.v tb_blocking_caveat.v
gtkwave blocking_caveat.vcd
```



**Evidence:** The later statement picks up the freshly-written value from its predecessor, not the value that existed when the block began.
**Finding:** This confirms, mechanically, why statement order is not a stylistic detail with `=` — it's functionally load-bearing. It's also exactly the trap that produces mismatch when blocking assignments end up inside sequential always blocks by mistake.

<img width="2125" height="1196" alt="image" src="https://github.com/user-attachments/assets/2e43ea2a-1a62-4a04-9672-a1c9f5103f62" />

---

## 🗃️ Case File Summary

| Case | Subject | Verdict |
|---|---|:---:|
| 04-A | MUX simulation | ✅ Clean |
| 04-B | MUX synthesis | ✅ Clean |
| 04-C | MUX stress test | ✅ Clean |
| 04-D | Faulty MUX symptom | ⚠️ Anomaly |
| 04-E | Faulty MUX root cause | ⚠️ Latch inferred |
| 04-F | Blocking order (sim) | 🔍 Mechanism confirmed |
| 04-G | Blocking order (synth) | ✅ Clean |
| 04-H | Blocking + past value | 🔍 Mechanism confirmed |

**Pattern across all eight cases:** the multiplexer that survived synthesis intact was the one where every input condition had an explicit assignment. The one that didn't produced a latch — a direct, traceable consequence, not a random tool quirk. The blocking-assignment cases show *why*: `=` writes take effect immediately and are visible to every subsequent line in the same block, which is exactly the property that makes incomplete or carelessly-ordered code dangerous the moment it leaves purely combinational territory.

---

## 📌 Ruling

Blocking (`=`) assignments belong in combinational `always` blocks, where their immediate, in-order execution matches designer intent. Non-blocking (`<=`) assignments belong in sequential `always` blocks, where the old value needs to persist across an entire block evaluation. Every unassigned output, in every branch, under any condition, should be treated as a latch waiting to be discovered — ideally in simulation, not in the fab.

---


### FILLED BY

**PALREDDY.SAI TEJASHWINI**
B.Tech – Electronics & Communication Engineering
Anurag University

