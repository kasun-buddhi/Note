[[2023.06.15]] 
## Youtube video 
!(Understanding and working with PUlP)[https://www.youtube.com/watch?v=27tndT6cBH0]

PULP is basically open source platform

All is open source. except some memory technology libraries.

##### PULPPissimo Architecture
Risc-v based advanced microcontroller
they have previous version PULPino very less complicated.

##### PULP Cluster Architecture
- 8 RISC-V multicore cluster
- shared FPU for efficient resources minimization
- Shared Instruction Cache
- Multi-Core event unit for barriers and clock management
- DMA for efficient L2 <--> L1 data transfers

## Core
PULP start with __RI5CY__ core. using __RI5CY__ developed several cores based on __RI5CY__. example 64-bit one is __Ariana__
following shows other cores 
 - Micro-riscy
 - Zero-riscy
 - RI5CY+FPU
 - Ariana

### RI5CY Processor
- 4-stage pipeline
- New FPU

#### PULP Cores Memory Interface
It is a custom interface that can be adapt to other interface like axi4, APB, ...

### Xpulp Extention
- ### Memory Access Extension

- ### Hardware loops extensions

- ### Bit manipulation extensions

- ### DSP extensions

- ### Packed-SIMD extension

## ALU architecture
- Advanced ALU for Xpulp extensions
- Optimized datapath to reduce resources 
- Multiple-adders for round
- Adder followed by shifter for fixed point normalization
- Clip unit uses one adder as a comparator and the main comparator

## MUL architecture

### 2D convolution with Xpulp extension

## PUPL cores Interrups 

### Wait for interrupt & Power manager (WFI)
WFI instruction disables the clock
- Dynamic power saved when core is in IDLE
- Taken or Not interrupt wake up the core that stats from the instruction after WFI 
- The core waits for all the inflight instructions before switching off the clock
- The pipeline and stage registers are clock gated when not used

### Performance Counter

[[2023.06.23]]
# GVSOC

the virtual platform must be launched through the common runner called pulp-run.

The specified platform must be gvsoc (through -platform) and the only mandatory option is either -config-file or -config in order to give the system configuration to simulate
As the other options for the platform must be given through the configuration file. 