	bne x4, x5, label
In traditional systems, you can branch to the data part also. 70% of the security problems nowadays are due to this lol. There are security measures built into the modern processor that prevents this. That is, the code cannot jump to the data part of the memory.

Normally you can jump +/- 4KiB


**Pseudo Instructions:**
	mv, rd, rs1
- This literally is addi rd, rs1, 0.
- This is not a native instruction though.

	li rd, CONSTANT
- Loads a constant value into the register 
- addi rd, x0, 0

	la rd, symbol 
- To load the address of a symbol.
- la rd, SYMBOL
- str .ascii "HELLO"
> lui rd, SYMBOL[31:12]
> addi rd, t0, SYMBOL[11:0]  

? Doubt = what does the upper thing do?


lui = load upper immediate. 


**Executables**
linux creates an executable of that name called an ELF (Executable Linked Format). (a.out)

PE in windows = Portable Executable

The executable itself contains many components. The executable is loaded into the memory and executed by the processor lol. It has multiple sections. But in the most basic form, 

		.text
		------
		.data


	.align 4 
It is used to align the thing that comes down to an address that is a multiple of 4.
\_start 

- Note that the register names are specified in RISC-V. I
- How you use the register is up to you. In order to ensure compatibility, you use the prescribe standard of using registers.

	lb t1, 0(t1)
It is used to store the data in address t1 to t1.

	beqz t1, 1f
1f is used to branch to the forward label 1.

	j 1b
This is a pseudo instruction. Here is means "jump" to the address that 1 backwards points to.

STANDARD EXECUTABLE CREATION:
	Assembly Language Program -> Object file -> Executable



	riscv64-unknown-elf-gcc -nostdlib -nostartfiles -T ./spike.lds strlen.S -o strlen.elf

-nostdlib -nostartfiles
	stdlib gives functionalities like printf scanf etc

startfiles are the files the processor starts executing
In intel, PC starts at 0xFFFF0. You need to have some some information at that partucular address. This is called **startup code** or **bootloader**. 

The linker description file indicates how to place files in memory.

>OUTPUT ARCH("riscv")
>ENTRY(_start) 
>SECTIONS
>{
>	.=  ....
>	.text : ALIGN(4K)
>	{ ...
>	}
>	.data: ALIGN(4K)
>	{...
>	}
>}

Generally, the linker description file is not created by us. gcc takes care of it automatically. 

FUN TASK: make gcc dump the linker descriptor script and take a look at it.
