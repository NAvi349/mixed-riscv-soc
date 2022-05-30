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

iv. Give the module name properly ```.tlv``` file

![image](https://user-images.githubusercontent.com/66086031/170980065-670f0c05-d426-4fc9-bd72-0c3fc1cf5c74.png)

v. ```sandpiper-saas -i pip_riscv_rv32I.tlv -o pip_riscv_rv32I.v --iArgs```

![image](https://user-images.githubusercontent.com/66086031/170980263-8fff8677-5403-4318-b7b2-75901b91d58b.png)

- The verilog files were successfully generated for my RISC-V Core.

vi. Instantiate the RISC-V core in the ```rvmyth_pll.v``` file

![image](https://user-images.githubusercontent.com/66086031/170980278-be7c8068-131a-4130-bbf8-388aa39c49ae.png)


We shall now verify the PLL.
![image](https://user-images.githubusercontent.com/66086031/170978505-eb76091b-fbbd-40e6-9748-b1a4f465d178.png)

##







