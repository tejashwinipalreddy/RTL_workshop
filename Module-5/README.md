

# ⚡ RTL Workshop | Module 5
# Latch Inference & Logic Optimization in Yosys Synthesis

*A hands-on investigation into how incomplete Verilog coding creates unwanted hardware*

![Verilog](https://img.shields.io/badge/HDL-Verilog-1f6f43?style=for-the-badge)
![Yosys](https://img.shields.io/badge/Synthesis-Yosys-2d5fa3?style=for-the-badge)
![SKY130](https://img.shields.io/badge/PDK-SKY130-a3312d?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

</div>

---

## 🧭 What This Lab Is About

Synthesis tools don't just translate Verilog into gates — they **interpret intent**. When an RTL description leaves a condition unhandled, Yosys doesn't guess; it inserts a **latch** to hold the last known value, because that's the only way to preserve simulation-consistent behavior. That single decision is often the difference between a clean combinational circuit and hardware nobody intended to build.

This lab walks through eleven small experiments — broken IF statements, broken CASE statements, working versions of each, and a few classic combinational blocks — to build an intuition for **when and why latches appear**, and how to design around them.

> 💡 **Central question this module answers:** *If I forget to assign an output somewhere in my always block, what does the synthesis tool actually do about it — and can I see it happen?*

---

## 🧪 Lab Setup

| Component | Role |
|---|---|
| Verilog HDL | Circuit description language |
| Icarus Verilog (`iverilog`) | Compiles & runs simulations |
| GTKWave | Waveform inspection |
| Yosys | RTL synthesis + schematic generation |
| SKY130 cell library | Standard-cell technology target |
| Ubuntu Linux | Host environment |

### Skills exercised
☑ Reading synthesis warnings and connecting them to RTL structure
☑ Recognizing latch inference in a generated schematic
☑ Writing fully-specified combinational `always` blocks
☑ Cross-checking simulation waveforms against synthesized netlists
☑ Building and verifying a multi-bit arithmetic circuit (Ripple Carry Adder)

---

## 🗂️ Lab Map

Rather than a flat list, the experiments are grouped by **what question each one answers**:

| Group | Question Being Tested | Experiments |
|---|---|---|
| **A. The IF Trap** | What happens when an `if` has no `else`? | 1, 2, 3, 4 |
| **B. The CASE Trap** | Does an incomplete `case` behave the same way? | 5, 6, 7, 8 |
| **C. Known-Good Combinational Blocks** | What does *correct* look like at synthesis? | 9, 10, 11 |
| **D. Findings** | What's the pattern across all of it? | Results, Conclusion |

---

# 🅰️ Group A — The IF Trap

## Experiment 1 · An IF With No Escape Route

**Setup:** A combinational block assigns the output only inside the `if` branch — there's no `else`.

```bash
iverilog -o incom_if incom_if.v incom_if_tb.v
gtkwave incom_if.vcd
`

**What the waveform shows:**
- Output updates while `sel` is asserted.
- The moment `sel` drops, the output **freezes** at its last value instead of resetting or changing.

**Why it matters:** This "freeze" is the simulator's way of modeling exactly what real latch hardware would do — it's not a bug in simulation, it's a preview of the synthesized circuit.

---

## Experiment 2 · Watching Yosys Make the Same Decision

**Setup:** Synthesize the identical design and inspect the schematic.

<img width="2235" height="1213" alt="image" src="https://github.com/user-attachments/assets/5633cbbc-327c-4f26-b5d8-01190b8bf0dc" />


<img width="2137" height="886" alt="image" src="https://github.com/user-attachments/assets/3a81915d-14a1-42f7-b442-551d21303f0c" />

```bash
yosys
read_verilog incom_if.v
synth -top incom_if
show
```



**What the schematic shows:** A **D-latch primitive** sitting where you'd expect a simple mux or buffer. Yosys had no choice — with one branch of the condition unassigned, holding the previous value is the only synthesizable interpretation.

**Takeaway:** Simulation *predicted* this. Synthesis *confirmed* it.

---

## Experiment 3 · A Closer Look at the Waveform

**Setup:** Re-run the same simulation, this time focusing purely on timing detail.

```bash
iverilog -o incom_if incom_if.v incom_if_tb.v
gtkwave incom_if.vcd
```



<img width="2156" height="1028" alt="image" src="https://github.com/user-attachments/assets/4f9191b4-8395-470e-9f8a-748c39ca8450" />

**Detail worth noting:** Every transition of the output aligns exactly with a transition of the driving condition — except when the condition goes false, where the output simply holds. This is the signature pattern to learn to spot at a glance.

---

## Experiment 4 · Does IF-ELSE Fix It? (Only If Complete)

**Setup:** Synthesize a second IF-based design, this time with an incomplete IF-ELSE structure.

```bash
yosys
read_verilog incom_if2.v
synth -top incom_if2
show
```

<img width="2167" height="1108" alt="image" src="https://github.com/user-attachments/assets/553690f0-7f8c-4a70-9516-36e14e83f6d8" />


**Finding:** Adding an `else` branch helps *only if every path is actually covered*. Partial coverage — even with an else present — still leaves gaps that resolve into latch storage.

---

# 🅱️ Group B — The CASE Trap

## Experiment 5 · An Incomplete `case`

```verilog
module incomp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
    always @(*) begin
        case(sel)
            2'b00 : y = i0;
            2'b01 : y = i1;
        endcase
    end
endmodule
```

Two of four possible `sel` values (`2'b10`, `2'b11`) are never handled.


**Diagnosis:** Same failure mode as the IF trap, different syntax. The netlist confirms a latch; the waveform confirms output-holding for the unhandled `sel` codes.

---

## Experiment 6 · The Fix: a `default` Branch

```verilog
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
    always @(*) begin
        case(sel)
            2'b00   : y = i0;
            2'b01   : y = i1;
            default : y = i2;
        endcase
    end
endmodule
```

<img width="2200" height="1097" alt="image" src="https://github.com/user-attachments/assets/53712ba2-711c-4372-837a-febb0b1a6437" />


**Diagnosis:** One `default` line eliminates every gap at once. The synthesized result is pure combinational logic — no latch primitive anywhere in the schematic.

> 🩹 **Rule of thumb:** In a `case` block, `default` is cheap insurance. Add it even when you're "sure" you've covered every value.

---

## Experiment 7 · A Sneakier Version: Partial *Signal* Coverage

```verilog
module partial_case_assign (input i0, input i1, input i2, input [1:0] sel,
                             output reg y, output reg x);
    always @(*) begin
        case(sel)
            2'b00 : begin y = i0; x = i2; end
            2'b01 : y = i1;
            default : begin x = i1; end
        endcase
    end
endmodule
```

This one has a `default` branch — and *still* infers latches. Why? Because each individual output signal needs full coverage, not just each case branch.


**Diagnosis:**
- `y` is never assigned in the `default` branch → latch on `y`.
- `x` is never assigned in the `2'b01` branch → latch on `x`.

**Takeaway:** "I have a default case" is not the same guarantee as "every output is assigned in every branch." Check signal-by-signal, not just branch-by-branch.

---

## Experiment 8 · A Fully-Specified 4-Way `case` (Sanity Check)

```verilog
module bad_case (input i0, input i1, input i2, input i3, input [1:0] sel, output reg y);
    always @(*) begin
        case(sel)
            2'b00 : y = i0;
            2'b01 : y = i1;
            2'b10 : y = i2;
            2'b11 : y = i3;
        endcase
    end
endmodule
``

**Diagnosis:** Despite the module's name, all four 2-bit combinations are explicitly listed, so there's actually nothing "bad" about its coverage — the waveform confirms clean, correct switching with no latch behavior.

---

# 🅲 Group C — Known-Good Combinational Blocks

## Experiment 9 · 2:1 Multiplexer



The select line cleanly routes one of two inputs to the output with no ambiguity — a textbook fully-covered combinational design.

## Experiment 10 · 1:4 Demultipxer

A single input is steered to exactly one of several outputs at a time, with all non-selected lines correctly held inactive — no accidental storage anywhere.

## Experiment 11 · 8-Bit Ripple Carry Adder



Carry propagates correctly stage-to-stage across all eight full-adder cells, and sum/carry outputs match expected binary addition for every tested input pair.

---

# 📈 Findings

## Pattern Recognition Table

| Symptom in Waveform | Symptom in Schematic | Underlying Cause |
|---|---|---|
| Output freezes instead of updating | Latch primitive appears | Missing `else` / missing `case` branch |
| Only *some* outputs freeze | Latch on specific signals only | One signal assigned in every branch, another isn't |
| No freezing, output always tracks inputs | Pure combinational gates only | Every input combination explicitly assigned |

## Scoreboard

| # | Design | Latch? | Root Cause / Why Not |
|---|---|:---:|---|
| 1–3 | Incomplete IF | ✅ | No `else` branch |
| 4 | Incomplete IF-ELSE | ✅ | Coverage still incomplete |
| 5 | Incomplete `case` | ✅ | Missing `sel` values |
| 6 | Complete `case` | ❌ | `default` closes all gaps |
| 7 | Partial signal assignment | ✅ | Per-signal coverage gap despite `default` |
| 8 | Fully-specified `case` | ❌ | All 4 combinations explicit |
| 9 | MUX | ❌ | Fully combinational |
| 10 | DEMUX | ❌ | Fully combinational |
| 11 | Ripple Carry Adder | ❌ | Fully combinational arithmetic |

**Synthesis optimizations observed across the module:** constant propagation, logic simplification, resource sharing, mux optimization, and efficient carry-chain arithmetic mapping — all while preserving functional equivalence to the RTL.

---

## 🧩 Closing Thoughts

Eight of eleven experiments in this lab exist purely to demonstrate *one* idea from different angles: **a synthesis tool cannot invent behavior you didn't specify — it can only preserve state when you leave it a gap.** Whether that gap comes from a missing `else`, a missing `case` branch, or a missing per-signal assignment inside an otherwise-complete branch, the outcome is identical — a latch shows up in silicon that was never meant to be there.

The fix, in every single case, was the same one-line habit: **assign every output, in every branch, for every possible input combination.**

Working through the correct designs (Experiments 6, 8–11) alongside the broken ones (1–5, 7) made the SKY130 synthesis flow feel less like a black box and more like a predictable translator — one that rewards precise, exhaustive RTL with clean, minimal hardware.

---


## ✍️ Author

**Palredd.Sai Tejashwini**
B.Tech – Electronics & Communication Engineering
Anurag University
