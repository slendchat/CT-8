# Processor: CT-8

# TABLE OF CONTENTS
- [Processor: CT-8](#processor--ct-8)
  * [DESCRIPTION](#--description--)
    + [Overview](#overview)
    + [Terminology](#terminology)
  * [MEMORY](#memory)
    + [Registers](#registers)
    + [Address layout](#address-layout)
    + [RAM banking](#ram-banking)
  * [INSTRUCTIONS](#instructions)
    + [Instruction layout](#instruction-layout)

## DESCRIPTION
+ This is working version of *CT-8* processor capable to perform **logical** and **arithmetical** operations.
+ Also it is able to operate **branch** instructions, as uncoditional as conditional. 
+ It is made using the bare logical units like AND gate, decoder, multiplexer. 
+ The archetype used to create *CT-8* is intel *i4004* and *i8008* with slight changes and simplifications.
+ Documents on **references** can be found in *CT-8/Datasheets/* directory. 
![CPU](https://github.com/slendchat/CT-8/blob/main/images/CPU.png?raw=true)
### Overview
+ 8-bit data word size.
+ 8-bit instruction word size.
+ 16 bit address bus.
+ Endianess - big endian.
+ Operations are register-register and immidiate. 
+ Oerations store result in accumulator registor(RA).
+ HL register can be written separately using LW, or by one instruction using LDA instr.
+ TEMP and FLAG register can't be written or read by user intentionally.

### Terminology
+ imm8 / imm16 	-- stands for immediate value, following instruction word in memory.
+ imm8  	-- 8 refers to length in bits of imm value.
+ GP reg 	-- general purpose register.
+ immBit 	-- bit that specify operation to be immediate or not

## MEMORY
### Registers
+ A : ACCUMULATOR 0x0
+ B : GP register 0x2
+ C : GP register 0x4
+ D : GP register 0x6
+ E : GP register 0x8
+ L : GP/LOW INDEX REGISTER 0xA
+ H : GP/HIGH INDEX REGISTER 0xC
+ F : FLAGS (only zero is used, cant be accessed by user)<br />
	- C:  Carry<br />
	- Z:  Zero<br />
	- N:  Negative<br />
	- O:  Odd<br />

### Address layout
YXXXXXXXXXXXXXXX <br />
Y -1bit  -> 0 = ROM; 1 = RAM <br />
X -15bit -> ADDR <br />

### RAM banking
0x0000...0x7fff - ROM <br /> 
0x8000...0xffff - RAM <br />

## INSTRUCTIONS

### Instruction layout
**Instruction layout is ZZZYXXXX** <br />
X: 4 bit instruction identifier<br />
Y: 0 if is imm, 1 if register<br />
Z: 3 bit register identifier

|HEX_CODE|code op | immBit | description |
| ------ | ------ | ------ | ----------- |
|10|0000 NOP|	 	  |				|
|11|0001 LW | 1	  |reg	 <-	[HL]			 |
|12|0010 SW | 1	  |[HL]  <-	reg				 |
|13|0011 MW | 1	  |reg	 <-	RA				 |
|14|0100 NOP| 1	  |							 |
|15|0101 JNZ| 1	  |PC 	 <- [HL] if Z == 0   |
|16|0110 JZ | 1	  |PC 	 <- [HL] if Z == 1 	 |
|17|0111 JMP| 1	  |PC	 <- [HL]			 |
|18|1000 ADD| 1	  |RA	 <- RA  +		reg  |
|19|1001 SUB| 1	  |RA	 <- RA  - 	reg|
|1a|1010 AND| 1	  |RA	 <- RA  and	reg|
|1b|1011 OR | 1	  |RA	 <- RA  or 	reg|
|1c|1100 XOR| 1	  |RA	 <- RA  xor	reg|
|1d|1101 NOT| 1	  |RA	 <- 	  not 	reg|
|1e|1110 CMP| 1	  |Z=1   <- RA  ==	reg|
|1f|1111 HLT| 1  	  |				|
||	 |	  |				|
|00|0000 NOP| 	  |				|
|01|0001 LW | 0	  |reg	   <-	imm8            |  
|02|0010 SW | 0	  |[imm16] <-	reg             |    
|03|0011 NOP| 0	  |				|
|04|0100 LDA| 0	  |HL    =  imm16               |
|05|0101 JNZ| 0	  |PC 	 <- imm16 if Z == 0     |         
|06|0110 JZ | 0	  |PC 	 <- imm16 if Z == 1     |         
|07|0111 JMP| 0	  |PC	 <- imm16                             |             
|08|1000 ADD| 0	  |RA	 <- RA 	+		imm8|
|09|1001 SUB| 0	  |RA	 <- RA 	- 		imm8|
|0a|1010 AND| 0	  |RA	 <- RA 	and		imm8|
|0b|1011 OR | 0	  |RA	 <- RA 	or 		imm8|
|0c|1100 XOR| 0	  |RA	 <- RA 	xor		imm8|
|0d|1101 NOT| 0	  |RA	 <- not 	imm8        |      
|0e|1110 CMP| 0	  |Z=1     <- RA == imm8          |    
|0f|1111 HLT| 0 	  |                             |

