# Processor: CT-8
### === **DESCRITPION** ===
+ This is working version of *CT-8* processor capable to perform **logical** and **arithmetical** operations.
+ Also it is able to operate **branch** instructions, as uncoditional as conditional. 
+ It is made using the bare logical units like AND gate, decoder, multiplexer. 
+ The archetype used to create *CT-8* is intel *i4004* and *i8008* with slight changes and simplifications.
+ Documents on **references** can be found in CT-8/Datasheets/ directory. 

### === OVERVIEW ===
+ 8-bit data word size
+ 8-bit instruction word size
+ 16 bit address bus
+ endianess - big endian
+ operations are register-register and immidiate. 

### === TERMINOLOGY ===
+ imm8 / imm16 	-- stands for immediate value, following instruction word in memory.
+ imm8  	-- 8 refers to length in bits of imm value.
+ GP reg 	-- general purpose register.
+ immBit 	-- bit that specify operation to be immediate or not
+ 

### === INSTRUCTIONS ===
|code op | immBit | description |
| ------ | ------ | ----------- |
|0000 NOP|	  |		|
|0001 LW | 1	  |reg	 <-	[HL]	|
|0010 SW | 1	  |[HL]  <-	reg				 |
|0011 MW | 1	  |reg	 <-	RA				 |
|0100 NOP| 1	  |							 |
|0101 JNZ| 1	  |PC 	 <- [HL] if Z == 0   |
|0110 JZ | 1	  |PC 	 <- [HL] if Z == 1 	 |
|0111 NOP| 1	  |							 |
|1000 ADD| 1	  |RA	 <- [RA]  +		[reg]|
|1001 SUB| 1	  |RA	 <- [RA]  - 	[reg]|
|1010 AND| 1	  |RA	 <- [RA]  and	[reg]|
|1011 OR | 1	  |RA	 <- [RA]  or 	[reg]|
|1100 XOR| 1	  |RA	 <- [RA]  xor	[reg]|
|1101 NOT| 1	  |RA	 <- 	  not 	[reg]|
|1110 CMP| 1	  |Z=1   <- [RA]  ==	[reg]|
|1111 HLT| 1  	  |							 |
|0000 NOP| 		  |								|	
|0001 LW | 0	  |reg	   <-	imm8            |  
|0010 SW | 0	  |[imm16] <-	reg             |    
|0011 MW | 0	  |not done,the same as the LW  |
|0100 LDA| 0	  |HL    =  imm16               |
|0101 JNZ| 0	  |PC 	 <- imm16 if Z == 0     |         
|0110 JZ | 0	  |PC 	 <- imm16 if Z == 1     |         
|0111 NOP| 0	  |                             |             
|1000 ADD| 0	  |RA	 <- [RA] 	+		imm8|
|1001 SUB| 0	  |RA	 <- [RA] 	- 		imm8|
|1010 AND| 0	  |RA	 <- [RA] 	and		imm8|
|1011 OR | 0	  |RA	 <- [RA] 	or 		imm8|
|1100 XOR| 0	  |RA	 <- [RA] 	xor		imm8|
|1101 NOT| 0	  |RA	 <- not 	imm8        |      
|1110 CMP| 0	  |Z     <- [RA]==imm8          |    
|1111 HLT| 0 	  |                             |





=== INSTRUCTION LAYOUT ===
Instruction layout is ZZZYXXXX 
X: 4 bit instruction identifier
Y: 0 if is memory, 1 if register
Z: 3 bit register identifier

=== ADDRESS LAYOUT ===
YXXXXXXXXXXXXXXX
Y -1bit  -> 0 = ROM; 1 = RAM
X -15bit -> ADDR
=== RAM BANKING ===
0x0000...0x7fff - ROM
0x8000...0xffff - RAM
