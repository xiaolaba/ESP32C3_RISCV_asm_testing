# ESP32C3_RISCV_asm_testing 
uses the riscv32-esp-elf-as.exe, came from esp32 core of Arduino IDE,  

exe file folder, may be changed upon ESP upgrade/update the tool/patch   
```
%userprofile%\.espressif\tools\riscv32-esp-elf\esp-2021r2-patch3-8.4.0\riscv32-esp-elf\bin\
```
exe file name,  
```
riscv32-esp-elf-as.exe
```

testing code,
[NOP.asm](NOP.asm)  
```
# RISCV assembler, comment uses (#), not usually found (;) of x86/avr assembler
# ref:
# https://github.com/riscv-non-isa/riscv-asm-manual/blob/master/riscv-asm.md
# xiaolaba, 2022-DEC-22, testing
# assembler used, 
# https://github.com/kcelebi/riscv-assembler


# NOP is a pseudoinstruction that expands to ADDI x0, x0, 0. The x0 (or zero) is a read-only register dedicated to the value zero, i.e., it is hardwired to zero for every single bit. Whatever is written to this register is simply discarded since its value can't be modified.
# From The RISC-V Instruction Set Manual Volume I: Unprivileged ISA:
#   The NOP instruction does not change any architecturally visible state, except for advancing the pc and incrementing any applicable performance counters. NOP is encoded as ADDI x0, x0, 0.
# Keeping in mind that RISC-V has no arithmetic flags (i.e., carry, overflow, zero, sign flags), any arithmetic operation whose destination register is x0 will do as a no operation instruction regardless of the source registers since the net result will consist of advancing the program counter to the next instruction without changing any other relevant processor's state.

#Register 	ABI 	Use by convention 	Preserved?
#x0 	zero 	hardwired to 0, ignores writes 	n/a

#capital_letter:
#	ADDI x1 x0 x0

#capital_letter:
#	ADD x1 x0 x0

#small_letter_ok:
	add x0,x0,0
	add x0,x0,0

#.section .rodata
prompt: .asciz "Value of t0 = %ld and value of t1 = %ld\n"
#.section .text
myfunc:
    addi    sp, sp, -8
    #sd      ra, 0(sp)
    la      a0, prompt
    mv      a1, t0
    mv      a2, t1
    call    printf
    #ld      ra, 0(sp)
    addi    sp, sp, 8
    ret
	.equ UART_BASE, 0x40003080
	lui	a0, %hi(UART_BASE)
	addi a0, a0, %lo(UART_BASE)
	
#.data
#.equ .string 	"string"
#.asciz 	"string" 	#emit string (alias for .string)
	.equ 	name, 10 	#constant definition

#.macro 	name arg1 [, argn] 	begin macro definition \argname to substitute
#.endm 		end macro definition

#.type 	symbol, @function 	accepted for source compatibility
#.option 	{rvc,norvc,pic,nopic,relax,norelax,push,pop} 	RISC-V options. Refer to .option for a more detailed description.
.byte 	0x55,0xaa #expression [, expression]* 	8-bit comma separated words
.2byte 	0x1111, 0xFFFF #expression [, expression]* 	16-bit comma separated words
#.half 	expression [, expression]* 	16-bit comma separated words
#.short 	expression [, expression]* 	16-bit comma separated words
#.4byte 	expression [, expression]* 	32-bit comma separated words
#.word 	expression [, expression]* 	32-bit comma separated words
#.long 	expression [, expression]* 	32-bit comma separated words
#.8byte 	expression [, expression]* 	64-bit comma separated words
#.dword 	expression [, expression]* 	64-bit comma separated words
#.quad 	expression [, expression]* 	64-bit comma separated words

```

assembly code,  
[a.bat](a.bat)  
```
riscv32-esp-elf-as.exe NOP.s -o NOP.elf
```
