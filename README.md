
![Mixed-Signal RISC-V](https://user-images.githubusercontent.com/66086031/171179064-be5a0f5f-7d81-48bb-8e34-e741f7c64097.png)

<!-- TOC start -->
- [Introduction](#introduction)
  * [Mixed-Signal Design](#mixed-signal-design)
  * [Verification](#verification)
- [RVMYTH RISC-V Core](#rvmyth-risc-v-core)
- [TL - Verilog](#tl---verilog)
  * [Timing Abstraction](#timing-abstraction)
- [ASIC Vs FPGA](#asic-vs-fpga)
- [Makerchip](#makerchip)
- [TLV to RTL](#tlv-to-rtl)
  * [Steps to convert TL - Verilog to System Verilog/verilog](#steps-to-convert-tl---verilog-to-system-verilogverilog)
- [iverilog Simulation](#iverilog-simulation)
  * [Assembly Program in the .tlv file](#assembly-program-in-the-tlv-file)
  * [iverilog flow](#iverilog-flow)
- [FPGA Flow](#fpga-flow)
  * [PLL](#pll)
  * [ILA ( Integrated Logic Analyzer )](#ila--integrated-logic-analyzer-)
- [RTL Simulation in Vivado](#rtl-simulation-in-vivado)
- [FPGA Synthesis](#fpga-synthesis)
- [Bit-stream Generation and Program FPGA Step](#bit-stream-generation-and-program-fpga-step)
- [Acknowledgements](#acknowledgements)
<!-- TOC end -->
## Introduction

![image](https://user-images.githubusercontent.com/66086031/170964933-988ad758-8094-4207-9052-456dd3baeae1.png)

### Mixed-Signal Design

- Analog IP and Digital IP on the same package (chip)
- In this workshop, **RVMYTH** is interfaced with a PLL **(avsdpll_1v8)**
- PLL acts as a multiplier here

### Verification
Verification is done in two parts
- RTL Flow (iverilog and GTKwave)
- FPGA Flow (Xilinx Vivado)

## RVMYTH RISC-V Core

- RV32I Instruction Set
- Written in TL - Verilog
- 5 staged Pipelined processor
  * Fetch Stage
  * Decode Stage
  * Execute Stage
  * Memory Access Stage
  * Register Write Back Stage 
- Data Hazards solved using Register Bypass (forwarding0 technique
- b - type and j - type instructions have two-cycle latency 

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
- In System Verilog/Verilog when we want to add more pipeline stages, we have to create more flip flops and also duplicate the signal declarations for each pipeline stage
- In TL - Verilog, we just have to move the computation to a different pipeline stage

Example - warp - V core created by Steve Hoover, founder of EDA, is a highly parameterized processor

## ASIC Vs FPGA

| ASIC                          | FPGA           |
| ---------------------         |:-------------: |
| Not reconfigurable            | Reconfigurable |
| Final Stage implementation    | Useful in prototyping a design|  
| Huge time required to design  | Less Time Comparatively      |


## Makerchip

## TLV to RTL

- **Icarus Verilog** - RTL Simulation Tool
- **GTKWave **- waveform viewer 
- Vivado HL Design Edition 

- SandPiper SaaS - converts to TLV to Verilog/System Verilog
- Installation link - https://pypi.org/project/sandpiper/

### Steps to convert TL - Verilog to System Verilog/verilog

i. ```git clone https://github.com/shivanishah269/risc-v-core.git```
ii. ```cd vsdfpga/verilog```

![image](https://user-images.githubusercontent.com/66086031/170985413-093cc1bc-83b5-446a-855d-6f628e3194b1.png)

<!--
iii. In this I replaced rvmyth.tlv with my **own TL - Verilog RISC-V Core**, which I designed as part of the 5 - Day RVMYTH Workshop.
 Details of it can be found here: https://github.com/NAvi349/riscv-myth-ws
 
![image](https://user-images.githubusercontent.com/66086031/170975385-49ab2cc6-4cb2-46a2-ab9e-ff28518fb1b3.png)

iv. Make some changes in the ```.tlv``` file to include the proper module name and assembly program for the counter

![image](https://user-images.githubusercontent.com/66086031/170981805-93df0f8f-10bf-4310-a5cb-2c9787dd3676.png)

v. ```sandpiper-saas -i pip_riscv_rv32I.tlv -o pip_riscv_rv32I.v --iArgs```

![image](https://user-images.githubusercontent.com/66086031/170982405-97cbd92c-36c1-4799-9187-3425580ec15a.png)

- The verilog files were successfully generated for my RISC-V Core.

vi. Instantiate the RISC-V core in the ```rvmyth_pll.v``` file

![image](https://user-images.githubusercontent.com/66086031/170982911-f3f301f4-18d6-430c-99a2-83c98aa896e2.png)

-->
We shall now verify the PLL.
![image](https://user-images.githubusercontent.com/66086031/170978505-eb76091b-fbbd-40e6-9748-b1a4f465d178.png)

## iverilog Simulation

### Assembly Program in the .tlv file

- This program basically generates a waveform whose values start from 0 to 255 and back again.

### iverilog flow

```console
iverilog rvmyth_pll_tb.v rvmyth_pll.v clk_gate.v 
./a.out
gtkwave rvmyth_pll.vcd

```
![image](https://user-images.githubusercontent.com/66086031/170985703-7400ec86-0ee8-4961-ab8c-fb9645aaa76d.png)

![image](https://user-images.githubusercontent.com/66086031/170987200-1c16d47a-a814-4f5a-bf5b-96b2f8ec07ff.png)


- The PLL output becomes the input to the RISC V Core.
- We can also the view the signal in analog format

![image](https://user-images.githubusercontent.com/66086031/170987365-20a455a8-2ffc-4efe-b578-3280afcd5c07.png)

![image](https://user-images.githubusercontent.com/66086031/170987474-56325ebf-99b7-4235-adb7-ce7210a85e07.png)

## FPGA Flow

![image](https://user-images.githubusercontent.com/66086031/170987645-1d58af17-7dc9-445b-979e-b04f63319791.png)

- PPLE is the Xilinx Vivado IP.

### PLL

i. Create a new project.

ii. Select the zedboard from the parts list.

![image](https://user-images.githubusercontent.com/66086031/170989639-abff4790-b92d-4ebf-8ce5-396eafb920c0.png)

iii. Add rvmyth.v, top_SoC.v and clk_gate.v as design sources

![image](https://user-images.githubusercontent.com/66086031/170989700-abf3b5d0-3918-4f94-8407-c0a51c276bef.png)
![image](https://user-images.githubusercontent.com/66086031/170989943-477d84eb-91f6-4031-a78c-e0b837324e09.png)

iv. Generate PLL and ILA IP from Xilinx Catalog

![image](https://user-images.githubusercontent.com/66086031/170990494-9116b07f-a26c-4437-b466-a84f56c21c6b.png)

v. Select PLL and 33 MHz as the input clock

![image](https://user-images.githubusercontent.com/66086031/170991098-a0bcdc05-89be-423c-8649-a1678414a03b.png)

![image](https://user-images.githubusercontent.com/66086031/170991052-a4a28dad-e60b-48ea-b8d5-1e490ee5171b.png)

vi. Enable override mode in PLLE2 Tab and compensation as BUF_IN 
![image](https://user-images.githubusercontent.com/66086031/170992657-f614fc0c-acd3-4e67-be4a-0e7554bb9412.png)

![image](https://user-images.githubusercontent.com/66086031/170991820-ce13bcb2-3d7d-460f-b799-7c679dc8f7ea.png)

vii. Click Finish

### ILA ( Integrated Logic Analyzer )

- It is used for monitoring internal signals in our design

i. Search ILA in the IP catalogue and select Integrated Logic Analyzer

ii. Set number of probes as 3 and sample depth as 131072

![image](https://user-images.githubusercontent.com/66086031/170993225-9b609307-ab09-4456-a6fe-f81d422f3a2a.png)

iii. Set probe length as 8 bits

![image](https://user-images.githubusercontent.com/66086031/170993270-60552a29-ce4f-4106-b5bb-1dd2e78f57e9.png)

iv. Click Finish

## RTL Simulation in Vivado

i. Add top_SoC_tb.v as source.

ii. Run RTL Simulation and run for 50000 units

![image](https://user-images.githubusercontent.com/66086031/170995605-c5df6b26-ed34-4ba2-8002-399605e005e6.png)

33 MHz clock is the input
10n MHz clock generated for Core

iii. Also view the DAC signal as analog format

![image](https://user-images.githubusercontent.com/66086031/170995805-ef6294e0-d240-4f7b-a5a9-290c1a603b5c.png)

## FPGA Synthesis

- Synthesis - mapping the netlist to the standard cell libraries or the actual the gates present in the FPGA
- Constraints file specifies pin mapping for simulation 

i. Add constraints file ```constraints.xdc```

![image](https://user-images.githubusercontent.com/66086031/170997290-cae30ba3-eb88-46fe-9646-4e9676f5230d.png)

ii. Run Synthesis

![image](https://user-images.githubusercontent.com/66086031/170997849-a67fb810-208c-4ff3-b5a3-f28c4573860a.png)

![image](https://user-images.githubusercontent.com/66086031/170999060-27c68713-b223-482d-b378-f4ec16c9f5fd.png)

iii. Now run Implementation

- Optimization
- Placing
- Routing

![image](https://user-images.githubusercontent.com/66086031/171000165-cbb1fd4c-bdd7-4cb8-884c-5db381795bc6.png)
- It fails to meet timing constraints

![image](https://user-images.githubusercontent.com/66086031/171000265-8d303193-4c77-4586-8642-ccb7a06f7ea6.png)

- Slack is negative here for hold

![image](https://user-images.githubusercontent.com/66086031/171000422-100188ca-2eb5-4171-975f-ea55de39f71f.png)

- It is actually due to a false path from PLL to ILA.

![image](https://user-images.githubusercontent.com/66086031/171000759-ae4d27db-24c2-41ca-b827-dfe8fe977b94.png)

iv. Solving Timing Issues

- Include these in the ```constraints.sdc``` file

```
set_false_path -hold -from [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to [get_pins uut3/inst/ila_core_inst/*D]
set_false_path -hold -from [get_pins uut1/inst/plle2_adv_inst/CLKOUT0] -to [get_pins uut3/inst/ila_core_inst/u_trig/u_trig/U_TM/N_DDR_MODE.G_NMU[2] .U_M/allx_typeA_match_detection.ltlib_v1_0_0_allx_typeA_inst/*/D]
```

![image](https://user-images.githubusercontent.com/66086031/171008893-bfdca8b1-3482-4991-bfc5-52893e9830cc.png)

v. Implementation Design

![image](https://user-images.githubusercontent.com/66086031/171008237-cfddaa6a-579c-4bd6-88ad-a05c204f00fd.png)

## Bit-stream Generation and Program FPGA Step

(This shall be done later after I obtain a Zedboard)

## Acknowledgements

[1] Kunal Ghosh - Founder, VSD

[2] Shivani Shah - Course Instructor







