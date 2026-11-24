A Bit - is 0v where there is no electron flow. The Bit 1 is +3.5v to +5v which does have electron flow. A transistor can regulate the electron flow based on voltage level.
A logic gate is a device that implements a boolean function. All computer operations are built from the combinations of logic gates which are just combinations of boolean functions.
Logic Gates accepts only binary inputs and binary outputs.
NAND or NOR gate is enough to implement all other logic gates.
Underlying every logic gate is a circuit called CMOS - Complementary MOSFET. CMOS consists of two complementary transistors, NMOS and PMOS. The simples CMOS circuit is an inverted or a NOT gate.

A Machine Instruction is a unique bit pattern that a device can indentify. Each Instruction is a small circuit that implements the instruction.

The 74HC00 Chip has 8 input pins, and it means accepts binary strings of 8 bits. The device has 4 output pins, and this means it produces binary strings of 4 bits from the 8 bit input
Even though its a small device it can take 2^8 + 2^4 = 272 binary strings.


Assembly language is the symbolic representation of binary machine code by giving bit patterns mnemonic names. We can build a device with 2 assembly instructions:
or <op1> <op2>
nand <op1> < op2>

Give each instruction an index in Binary Code, called operation code, or opcode for short, and embed it as a part of input. The Value for each instruction is specified as in table 2.3.4

Table 2.3.4: Instruction - OPcode mapping
Instruction: nand (Binary Code: 00)
Instruction: or (Binary Code: 01)

Now every input starts off with additional data at the beginning which is an opcode. For example: nand 1100,1100 which is 0011001100.
Because the starting opCode is 00 the decoder knows it should be sent to NAND device for computing.  
When we write assembly code and save in a text file, a program caleld an assembler translates the text file into binary strings that a device can understand.
A storage device is a  set of array of circuits for saving 0 and 1 states. A storage device can be anything that can store 0 and 1 and is retrievable.
The process: Fetch an instruction from a storage device --> Decode the instruction --> Execute the instruction

A program that translates such text forms to machine code is called a compiler.
CODE --> Compiler --> ASM
