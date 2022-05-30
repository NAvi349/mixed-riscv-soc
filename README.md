## Introdution

![image](https://user-images.githubusercontent.com/66086031/170964933-988ad758-8094-4207-9052-456dd3baeae1.png)

### Mixed Signal Design

- Analog IP and Digital IP on the same package (chip)
- In this workshop, **RVMYTH** is interfaced with a PLL **(avsdpll_1v8)**
- PLL acts a multiplier here

### Verification
Verification is done in two parts
- RTL Flow (iverilog and gtkwave)
- FPGA Flow (Xilinx Vivaldo)

## RVMYTH RISC-V Core

- RV32I Instruction Set
- Written in TL - Verilog
- 5 staged Piplined processor
  * Fetch Stage
  * Decode Stage
  * Execute Stage
  * Memory Access Stage
  * Register Write Back Stage 
- Data Hazards solved using Register Bypass (forwarding0 technique
- b - type and j - type instructions have two cycle latency 

## TL - Verilog

- Design is done at the Transaction Level MOdelling
- It is an extension of TL-X
- TL - Verilog makes it easier to design complex 

### Timing Abstraction

```verilog
|example
  @1
    ---pipeline-stage-1---
  @2
    ---pipeline-stage-2---   
  @3
    ---pipeline-stage-3---
```

- TL - Verilog makes retiming extremely easy.
- In System Verilog/Verilog when we want to added more pipeline stages, we have to create more flip flops and also duplicate the signal declarations for each pipeline stage
- In TL - Verilog, we just have to move the computation to a different pipeline stage
