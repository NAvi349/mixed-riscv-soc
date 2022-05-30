## Introdution

![image](https://user-images.githubusercontent.com/66086031/170964933-988ad758-8094-4207-9052-456dd3baeae1.png)

### Mixed Signal Design

- Analog IP and Digital IP on the same package (chip)
- In this workshop, **RVMYTH** is interfaced with a PLL **(avsdpll_1v8)**
- PLL acts a multiplier here

### Verification
Verification is done in two parts
- RTL Flow (iverilog and GTKwave)
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

Example - warp - V core created by Steve Hoover, founder EDA, is a highly parameterized processor

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
- Vivaldo HL Design Edition 

- SandPiper SaaS - converts to TLV to Verilog/System Verilog
- Installation link - https://pypi.org/project/sandpiper/

### Steps to convert TL - Verilog to System Verilog/verilog

i. ```git clone https://github.com/shivanishah269/risc-v-core.git```
ii. ```cd vsdfpga/verilog```

iii. In this I replaced rvmyth.tlv with my **own TL - Verilog RISC-V Core**, which I designed as part of the 5 - Day RVMYTH Workshop.
 Details of it can be found here: https://github.com/NAvi349/riscv-myth-ws
 
![image](https://user-images.githubusercontent.com/66086031/170975385-49ab2cc6-4cb2-46a2-ab9e-ff28518fb1b3.png)

iv. Make some changes in the ```.tlv``` file to include proper module name and assembly program for the counter

![image](https://user-images.githubusercontent.com/66086031/170981805-93df0f8f-10bf-4310-a5cb-2c9787dd3676.png)

v. ```sandpiper-saas -i pip_riscv_rv32I.tlv -o pip_riscv_rv32I.v --iArgs```

![image](https://user-images.githubusercontent.com/66086031/170982405-97cbd92c-36c1-4799-9187-3425580ec15a.png)

- The verilog files were successfully generated for my RISC-V Core.

vi. Instantiate the RISC-V core in the ```rvmyth_pll.v``` file

![image](https://user-images.githubusercontent.com/66086031/170982911-f3f301f4-18d6-430c-99a2-83c98aa896e2.png)


We shall now verify the PLL.
![image](https://user-images.githubusercontent.com/66086031/170978505-eb76091b-fbbd-40e6-9748-b1a4f465d178.png)

## iverilog Simulation

### Assembly Program in the .tlv file

- This program basically generates a waveform which oscillates from 0 to 255 and back again.









