# RTL Design of a 16-bit ALU

A 16-bit Arithmetic Logic Unit written in Verilog. Logical, arithmetic and multiplication operations are built as separate modules and integrated into one ALU that selects the operation from an opcode. The design was verified in simulation and synthesized in Xilinx Vivado.

## Block diagram

<img width="819" height="460" alt="ALU block diagram" src="https://github.com/user-attachments/assets/1fd32d8a-1f3b-4c5d-866f-f448c36f1d1a" />

| Signal | Width | Description |
|---|---|---|
| `A`, `B` | 16-bit | Input operands |
| `OP` | - | Opcode that selects the operation |
| `OUT` | 32-bit | Result (wide enough to hold the 16 × 16 multiplier output) |

Two demuxes route `A` and `B` to the logical, arithmetic or multiplier unit based on `OP`, and a mux picks the matching result for `OUT`. The mux and demuxes are written with `case` statements.

## Modules

**Logical unit** is behavioural code using a `case` statement. It supports invert, AND, OR, XOR, NAND, NOR, shifts and rotates (left and right).

**Arithmetic unit** is built from four 4-bit ripple-carry adders (RCA) with four 4-bit carry-lookahead (CLA) generators:

- RCAs save area, while CLAs save time.
- The CLAs only generate carries, so they add little area. They also stop each RCA from waiting on the previous block's ripple.
- RCA 1 produces the first 4 sum bits and CLA 1 produces the carry-in for RCA 2. The same pattern continues through the third and fourth blocks, balancing delay and area.

**Multiplier** is behavioural code using a `for` loop.

## Tools and flow

- **Language:** Verilog
- **Tool:** Xilinx Vivado (simulation and synthesis)

To run it, open `RTL_Design_of_16_bit_ALU.xpr` in Vivado, then run the behavioural simulation and the synthesis.

## Results

Simulation waveforms show correct outputs for the logical, arithmetic and multiplication operations across different input combinations. Synthesis produces a hardware implementation that can target an FPGA. See `ALU_PROJECT_REPORT.pdf` for the simulation waveform and synthesized schematic.

## Future work

- Optimize for performance, area and power
- Add division, floating point and pipelining
- Run the design through the ASIC flow with OpenLane
