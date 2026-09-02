# Design-and-Verification-of-AMBA-APB3-Protocol
# Wallace Tree Multiplier (16×16-bit) — File Summaries

Verilog HDL implementation of a 16-bit Wallace Tree Multiplier, designed and verified in Xilinx VIVADO. Below is a per-file summary suitable for individual file headers or a GitHub repo README.

**half_adder.v**
---
Module: half_adder Structural half adder used throughout the Wallace Tree reduction stages whenever a column has exactly two bits to combine. Built from a single XOR gate (sum) and AND gate (carry) — no sequential logic. Minimizes gate count and delay compared to using a full adder where only two inputs exist.

Inputs: a, b

Outputs: sum, carry

**full_adder.v**
---
**Module:** full_adder Structural 3-input full adder — the core compression element of the Wallace Tree. Reduces any three partial-product bits of equal weight down to two (sum + carry), which is what drives the tree's logarithmic reduction and speed advantage over sequential (array) addition. Built from two XOR/AND stages plus an OR gate (5 gates total, ~2-gate delay).

Inputs: a, b, cin

Outputs: sum, carry

**pp_gen.v**
---
**Module:** pp_gen Partial Product Generator. Uses a generate/AND-gate array to compute all 256 partial products (a[i] & b[j]) for a 16×16 multiplication. This 16×16 matrix of bits feeds directly into the Wallace Tree reduction module.

Inputs: a[15:0], b[15:0]

Outputs: pp[15:0][15:0] (partial product matrix)

**Wallace_tree_16bit.v**
---
**Module:** Wallace_tree_16bit The reduction core of the design. Takes the 16×16 partial product matrix and compresses it column-by-column using instantiated full_adder (3:2 compressor) and half_adder (2:2 compressor) modules across multiple stages, until only two rows (sum and carry) remain. This parallel, tree-structured compression is what gives the Wallace Tree its ~O(log n) delay versus O(n) for a ripple/array approach.

Inputs: pp[15:0][15:0]

Outputs: sum[31:0], carry[31:0]

**final_adder.v**
---
**Module:** final_adder 32-bit ripple-carry adder that performs the final carry-propagate addition of the Wallace Tree's two reduced output rows (sum and carry) to produce the final 32-bit product. Built structurally from chained full_adder instances.

Inputs: sum[31:0], carry[31:0]

Outputs: product[31:0]

**Wallace_multiplier_top.v**
---
**Module:** Wallace_multiplier_top Top-level integration module. Instantiates pp_gen → Wallace_tree_16bit → final_adder in sequence to form the complete 16×16 Wallace Tree Multiplier, producing a 32-bit product from two 16-bit operands.

Inputs: a[15:0], b[15:0]

Outputs: product[31:0]

**tb_Wallace_multiplier.v**
---
**Module:** tb_Wallace_multiplier (testbench) Verification testbench for Wallace_multiplier_top. Applies several stimulus vectors (e.g., 15×10, 255×5, 1234×56, 0×32767), dumps waveforms to Wallace_tree.vcd, and displays operand/product values via $display to confirm functional correctness in simulation.

**Reference:** Array Multiplier (comparison baseline)
---
A conventional array multiplier (AND-gate partial products + ripple-carry-style half/full adder array) was also implemented and simulated purely as a performance baseline — to contrast propagation delay, area, and structure against the Wallace Tree design (see Chapter 5 of the report, "Comparison with Other Multipliers").
Reference: Array Multiplier (comparison baseline)

A conventional array multiplier (AND-gate partial products + ripple-carry-style half/full adder array) was also implemented and simulated purely as a performance baseline — to contrast propagation delay, area, and structure against the Wallace Tree design (see Chapter 5 of the report, "Comparison with Other Multipliers").
