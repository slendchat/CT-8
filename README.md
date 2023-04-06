# Processor: CT-8
## === **DESCRITPION** ===
This is working version of *CT-8* processor capable to perform **logical** and **arithmetical** operations.
Also it is able to operate **branch** instructions, as uncoditional as conditional. 
It is made using the bare logical units like AND gate, decoder, multiplexer. 
The archetype used to create *CT-8* is intel *i4004* and *i8008* with slight changes and simplifications.
Documents on **references** can be found in CT-8/Datasheets/ directory. 

## === OVERVIEW ===
! 8-bit data word size
! 8-bit instruction word size
! 16 bit address bus
! endianess - big endian
! operations are register-register and immidiate. 

## === TERMINOLOGY ===
! 
=== INSTRUCTIONS ===
code op   immBit    RETURN
0	0000 NOP

1	0001 LW  	1		reg	 <-	[HL]
2	0010 SW  	1		[HL] <-	reg
3	0011 MW  	1		reg	 <-	RA	

4	0100 NOP	1
5	0101 JNZ	1		PC 	 <- [HL] if Z == 0
6	0110 JZ 	1		PC 	 <- [HL] if Z == 1

7	0111 NOP	1		

8	1000 ADD 	1		RA	 <- [RA] 	+		  [reg]
9	1001 SUB	1		RA	 <- [RA] 	- 		[reg]
a	1010 AND	1		RA	 <- [RA] 	and		[reg]
b	1011 OR 	1		RA	 <- [RA] 	or 		[reg]
c	1100 XOR	1		RA	 <- [RA] 	xor		[reg]
d	1101 NOT	1		RA	 <- not 	      [reg] 
e	1110 CMP 	1		Z=1  <- [RA]==[reg]

f	1111 HLT	1
=======================================================

code op   immBit    RETURN
0	0000 NOP
	
1	0001 LW  	0		reg	 	  <-	imm8
2	0010 SW  	0		[imm16] <-	reg
3	0011 MW  	0		reg	 	  <-	imm8	??????	- not done because it is the same as the LW
	
4	0100 LDA	0		HL    =  imm16
5	0101 JNZ	0		PC 	 	<- imm16 if Z == 0
6	0110 JZ 	0		PC 	 	<- imm16 if Z == 1
	
7	0111 NOP	0		
	
8	1000 ADD 	0		RA	 <- [RA] 	+		imm8
9	1001 SUB	0		RA	 <- [RA] 	- 		imm8
a	1010 AND	0		RA	 <- [RA] 	and		imm8
b	1011 OR 	0		RA	 <- [RA] 	or 		imm8
c	1100 XOR	0		RA	 <- [RA] 	xor		imm8
d	1101 NOT	0		RA	 <- not 	imm8
e	1110 CMP 	0		Z    <- [RA]==imm8
	
f	1111 HLT	0 
=================================================================================
= imm8/16 are the byte(s) immediately following the instruction byte in memory	=
= with imm8/reg and imm16/HL, the choice is indicated by the y-bit				      =
= (see INSTRUCTION LAYOUT)													                          	=
=================================================================================

=== REGISTERS ===
A : ACCUMULATOR
B : TEMP
C : GP register
D : GP register
E : GP register
L : GP/LOW INDEX REGISTER
H : GP/HIGH INDEX REGISTER
F : FLAGS (only zero is used)
	C:CARRY
	Z:ZERO
	N:NEGATIVE
	O:Odd



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
