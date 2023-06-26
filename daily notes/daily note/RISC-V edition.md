While programmers could ignore the advice and rely on computer architectures, compiler writers, and silicon engineers to make their programs run faster or be more energy-efficient without change. that era is over. For programs to changes 

[[2023.06.21]]
## Instruction Language of the Computer

The chosen instruction set is RISC-V, which was originally developed by UC Berkerly starting in 2010.
To demonstrate instruction sets we can look at two other popular instruction sets.

	1. MIPS is an elegant example of the intruction sets designed since 1980s. In several respects, RISC-V follows a similar designs.
	2. The Intel x86 originated in the 1970s, but still today powrs both the PC and the Cloud of the post-PC era.

This similarity of instructions sets occurs because all computers are constructed from hardware technologies. based on similar underlying and because there a few basic operations that all computers must provide. computer designers have common goals which are find a language that makes it easy to build the hardware and compiler while maximizing performance and minimizing cost and energy.

### Operations of the Computer Hardware

Operands is more complicated than hardware for a fixed number. This situation illustrates the first of three underlying principles of hardware designs.
__Design principle__ 1: Simplicity favors regularity

### Operands of the Computer Hardware
[[2023.06.23]]

__Design principle__ 2: Smaller is faster
A very large number of registers may increase the clock cycle time simply because it takes electronic signals longer when they must travel farther.
Guidelines such as "smaller is faster" are not absolute. it means 31 registers may not be faster than 32. 

[[2023.06.26]]
#### Compiling a C Assignment using Registers
```C
f = (g+h)-(i+j);
```
the variables f,g,h,i,j are assigned to the registers x19, x20, x21, x22 and x23. (register numbers come with 'x')

```asm
add x5, x20, x21
add x6, x22, x23
add x19, x5, x6
```

### Memory operands
The processor can keep only a small amount of data in registers, but computer memory contians billions of data elements. Hence, data structures(arrays and structures ) are kept in memory.
Arithmetic operations occur only on registers in RISC-V. thus, RISC-V must include instructions that transfer data between memory and registers. Such instruction are called data transfer instructions. memory address. Memory is just large, single dimension array, with the address acting as the index of the array, starting at 0. 

![[Screenshot from 2023-06-26 15-11-01.png]]

The data transfer instruction that copies data from the memory to a registers is traditionally called _load_. the real RISC-V name for this instruction is __ld__ (load doubleword)

The instruction complementary to load is traditionally called _store_. it copies data from a register to memory. 

#### Compiling using load and store

Assume h is associated with x21 and the base address of the array A is in x22. What is the RISC-V assembly code for the C assignment statement below
```c
A[12] = h + A[8];
```

Although there is a single operation is the C statement, 