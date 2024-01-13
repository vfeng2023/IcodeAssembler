I designed a symbolic language specification for the University of Virginia's CS 2130 Computer Systems and Organization binary icode.
***SYNTAX***
(LABELNAME) adds a marker to the immediately following command. Implements goto logic. The labels are not part of the program itself, but allow for a richer control over program flow compared to the do-while loop
COMMAND DEST, SOURCE
Dest and source should be register addresses

0 - LOAD dest, source (convert to int) rA = rB

1 ADD dest, addend rA += rB

2 AND dest, other rA&=rB

3 LOADM dest, memaddress register rA = read from memory at address rB

4 WRITE SRC, MEMADDRESS write rA to memory at address rB

5 OP dest, OPTYPE
BITNOT = 0  rA = ~rA
NEG = 1	rA = -rA
LGNOT = 2 rA = !rA
SAVEPC = 3 rA = pc

6 MOVE dest, TYPE, VALUE
RD = 0 rA = read from memory at pc + 1
PLUS = 1 rA += read from memory at pc+1
AND= 2 rA &= read from memory at pc+1
RDA = 3 rA = read from memory at address stored at pc+1
Only positive hex values are allowed. Negative values must be given in decimal format. 

7 JLE  compval, loc –Compare rA (as an 8-bit 2’s-complement number) to 0; – if rA <= 0, set pc = rB - otherwise, increment pc like normal.

HALT - automatically maps to 100000
Comments described by //

LOAD 2, 
MOVE 2, RD, 0x23
LOAD 1,2
MOVE 0, RD, 0
WRITE 1, 0
HALT

DESIGN
Parser that reads line by line ignoring comments
Splits into three parts: command + arguments
Command follows format of OPERATION a, b (which can be numbers or values) [+optional 3rd]
A is always an integer
B can be an integer or string
C is optional, but can be included. Only used for MOVE
