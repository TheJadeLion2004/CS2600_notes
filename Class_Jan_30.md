- Data -> Register -> Operations
- Heading: What is the size of the memory that is supported?
	- Memory size depends on the addressing of the memory that is allowed, here 32 bits
	- Extending the memory addressable?: Extending the range of bits from 32 bits to 39 bits.The size of the register does not determine the size of the memory that can be accessed. Done by Intel in 2000s.
	- QUIZ Question!!! Size of memory that can be addressed does not depend on the *size of the register* but the *size of the bus!*
	- Suppose you TOTALLY need a memory of more size than available in RAM. The OPERATING SYSTEM does this. The OS facilitates this essentially. 
		- 0xBAD ERROR in spike is kinda due to this (Improper addressing ie)


imm[11:0] ||  rs1 || funct3 || rd || opcode

	Imm -2048 to +2047 

	lw rd, imm(rs)

	lb rd, imm(rs) = performs sign extension, 8 bits. imm has 12 bits. (2's    complement)

	lbu rd, imm(rs) = loads 8 bits without sign extension. imm has 12 bits.

STORE INSTRUCTIONS:
Writes 32 bits to memory.

	sw rs, imm(rd)

Observe that there are at least 65 signals connecting the processor to the memory. 
Like 32(DATA) + 32 (ADDRESS) + 1(ENABLE READ/ WRITE)

> Load has an "I - type format"
> Store has an "S-type format"

You need to specify two registers.


Consider the following program:

	addi x1, x0, 0xABCDEF00
	sw x1, 4(x2)
	lb x3, 5(x2)
	lbu x4, 5(x2)  


(Random note: lhu = load half word unsigned)
How do you store the information? 0xABCDEF00
0x104 |0x105| 0x106| 0x107
AB       |CD      | EF      | 00        -> BIG ENDIAN
00       |EF       | CD     | AB        -> LITTLE ENDIAN

What is the problem with this?
	> Let's say you open a binary file from one machine to another following different endianness despite following the same RISCV architecture
	> Then all data will be reversed and that is tragic.

x2 + 5 = 105
x3 will contain FFFFFFEF (MSB is 1. So, it simply extends it with F's (1's))
x4 will contain 000000EF

**Aligned vs non aligned memory access**
Loads and stores are typically optimised for aligned memory access (I can only access memory in multiples of 4). 
- A6 A7 A8 A9 is non aligned
- A16 A17 A18 A19 is aligned 
Some processors support aligned memory access. However many processors also flat out refuse to support this. When you start looking into cores, this will make a lot more sense.

Similarly, we also have ALIGNED HALF WORD ACCESS (multiple of 2 bytes).

We however do not have any NON ALIGNED BYTE coz memory is byte addressable and therefore all bytes are aligned lol.

*******

**The Program Counter**
- Each program counter call typically increments it by 4 (Since each instruction is 32 bits long)
- However, for loops and stuff, or for conditional branch instructions, it can move to specific locations in the memory.

**Branch Instructions**

	bne x4, x5, label
label is the offset with respect to label.
> if x4 != x5
> 	then 
> 		pc <- pc + label
> 	else
> 		pc <- pc + 4

label is an immediate value and can be of 12 bits.

	imm[12|10:5] | rs2 | rs1 | funct3 | imm[4:1|11] | opcode    -- B-type
Branch range: +/- 4KiB for beq, bne, blt, bge
more than that: bltu, bgeu +8KiB

Can only jump to even addresses. Observe that bit 0 is not there. This is  because we want the program counter to jump to instructions. 
You might ask, each instruction is 32 bits long, why not optimise more and remove another bit?
The thing is, RISCV supports compressed instructions sets where each instruction is 16 bits long. Thus this artifact.


