# SoftCore CPU
This project was part of my 3rd year graduation in Electrical and electronic engeneering major.The CPU part has been implemented using VHDL in Quartus II 13.01 and the Assembler using VB.NET in Visual Studio 2013. The CPU has 32bit shared address/data bus with one byte addressing mode RAM,The implementation Architecture was based on Von Neumann architecture so it consists of registers, ALU (Arithmetic Logic Unit) and memory which are all connected to the same 32 bit bus along with Control Unit which controls the flow of data by control signals.
To read the full report:
https://github.com/AymenSekhri/Softcore-CPU/blob/master/repport.pdf
## Assembler
The only way to write program to the CPU is by writing the code then manually translating each instruction and its operands into the corresponding machine code then send the generated bytes to the CPU via transmission software by RS232 port. This task is pretty much exhaustive when it comes to writing tens lines programs, thus, an assembler has been implemented to overtake this task and convert the assembly code directly into machine code and upload it to the CPU without tiring the user to know any information about instructions machine codes.

### Comments
It is possible to insert comments anywhere the code by using double forward slash “//”, the next words of line will be commented (colored by dark green) and not translated into machine code.
### Labels
Since the address of certain instruction or data is dependent on the previous instructions, any modification of the first instructions must be followed with updating all addresses, here is where labels are useful they can allow the user to avoid dealing with addresses in any way. A label can be defined by using any name followed by a colon then the instruction or data desired to address. The name of the label must be alphanumeric characters or an underscore. To use the label, the user must write the @ symbol followed by the name of the label.


![GitHub](https://github.com/AymenSekhri/ISoftcore-CPU/blob/master/Images/1.png)


### Data Definitions
Sometimes programmers needs to define a certain constants or strings to be used by the program, this assembler allows to use 3 types of data: bytes, integers and strings. The bytes are simple 8bit numbers that can be defined by DB keyword followed by the bytes, it is possible to define array of bytes by separating them with spaces or tabs. The integers are 32bit numbers which can be defined by the keyword DD, it is possible to define an array with the same way as bytes too. Finally, Strings can be defined using keyword DS followed by the ASCII string between double quotes (note that the strings are null terminated). 


![GitHub](https://github.com/AymenSekhri/ISoftcore-CPU/blob/master/Images/2.png)


## The CPU Instruction Set


| Code  | Addressing Mode | Size   |
|-------|-----------------|--------|
| m32   | Memory Address  | 32 bit |
| imm32 | Immediate Value | 32 bit |
| r4    | Register        | 4 bit  |


| Instruction      | Opcode | Size | Architecture  | Cycles \- 16 | Notes                        |
|------------------|--------|------|---------------|--------------|------------------------------|
| MOV r4,imm32     | 0x01   | 6    | 0x01/r4/imm32 | 1            |
| MOV r4,r4        | 0x02   | 2    | 0x02/r4r4     | 1            |
| MOV r4,m32       | 0x03   | 6    | 0x03/r4/m32   | 6            |
| MOV r4,\[r4\]    | 0x04   | 2    | 0x04/r4r4     | 6            |
| MOV \[r4\],r4    | 0x05   | 2    | 0x05/r4r4     | 5            |
| MOV \[r4\],imm32 | 0x14   | 6    | 0x05/r4r4     | 5            |
| ADD r4,r4        | 0x06   | 2    | 0x6/r4r4      | 2            |
| SUB r4,r4        | 0x07   | 2    | 0x7/r4r4      | 2            |
| MUL r4,r4        | 0x08   | 2    | 0x8/r4r4      | 2            |
| AND r4,r4        | 0x09   | 2    | 0x9/r4r4      | 2            |
| OR r4,r4         | 0x0A   | 2    | 0xA/r4r4      | 2            |
| CMP r4,r4        | 0x0B   | 2    | 0xB/r4r4      | 2            | A<\- Oparand1  B<\-Oparand2  |
| ADD r4,imm32     | 0x0C   | 6    | 0xC/r4/imm32  | 2            |
| SUB r4,imm32     | 0x0D   | 6    | 0xD/r4/imm32  | 2            |
| MUL r4,imm32     | 0x0E   | 6    | 0xE/r4/imm32  | 2            |
| AND r4,imm32     | 0x0F   | 6    | 0xF/r4/imm32  | 2            |
| OR r4,imm32      | 0x10   | 6    | 0x10/r4/imm32 | 2            |
| CMP r4,imm32     | 0x11   | 6    | 0x11/r4/imm32 | 2            |
| ROR r4,imm32     | 0x12   | 6    | 0x12/r4/imm32 | 2            | Accepts values 1,8,16 and 24 |
| ROL r4,imm32     | 0x13   | 6    | 0x13/r4/imm32 | 2            | Accepts values 1,8,16 and 24 |
| INC r4           | 0x15   | 2    | 0x15/r4\.00   | 2            |           
| DEC r4           | 0x16   | 2    | 0x16/r4\.00   | 2            |
| PUSH r4          | 0x20   | 2    | 0x20/00\.r4   | 6            |
| PUSH imm32       | 0x21   | 5    | 0x21/imm32    | 6            |
| POP r4           | 0x22   | 2    | 0x22/r4\.00   | 7            |
| JMP m32          | 0x30   | 5    | 0x30/m32      | 1            |
| JMP r4           | 0x31   | 2    | 0x31/r4       | 1            |
| CALL m32         | 0x23   | 5    | 0x23/m32      | 8            |
| CALL r4          | 0x24   | 2    | 0x24/00\.r4   | 8            |
| RET              | 0x22   | 2    | 0x22/4\.00    | 7            | RET = POP IP                 |
| JZ  r4           | 0x40   | 5    | 0x40/m32      | 1            |
| JS  r4           | 0x41   | 5    | 0x41/m32      | 1            |
| JO  r4           | 0x42   | 5    | 0x42/m32      | 1            |
| JL  r4           | 0x43   | 5    | 0x43/m32      | 1            |
| JG  r4           | 0x44   | 5    | 0x44/m32      | 1            |
| JNZ r4           | 0x45   | 5    | 0x45/m32      | 1            |
| JNS r4           | 0x46   | 5    | 0x46/m32      | 1            |
| JNO r4           | 0x47   | 5    | 0x47/m32      | 1            |
| JZ  m32          | 0x48   | 2    | 0x48/00\.r4   | 1            |
| JS  m32        | 0x49   | 2    | 0x49/00\.r4   | 1            |
| JO  m32        | 0x4A   | 2    | 0x4A/00\.r4   | 1            |
| JL  m32        | 0x4B   | 2    | 0x4B/00\.r4   | 1            |
| JG  m32        | 0x4C   | 2    | 0x4C/00\.r4   | 1            |
| JNZ m32        | 0x4D   | 2    | 0x4D/00\.r4   | 1            |
| JNS m32           | 0x4E   | 2    | 0x4E/00\.r4   | 1            |
| JNO m32             | 0x4F   | 2    | 0x4F/00\.r4   | 1            |
| OUT Port,r4      | 0x50   | 3    | 0x50/r4/imm8  | 1            |
| NOP              | 0x90   | 1    | 0x90          | 1            |
| HLT              | 0xF0   | 1    | 0xF0          | 1            |

## Registers Codes

| Register | Code | Description              |
|----------|------|--------------------------|
| AX       | 0    | General Purpose Register |
| BX       | 1    | General Purpose Register |
| CX       | 2    | General Purpose Register |
| DX       | 3    | General Purpose Register |
| IP       | 4    | Instruction Pointer      |
| SP       | 5    | Stack Pointer            |
| BP       | 6    | Base Pointer             |


## I/O Peripherals Addresses

| Port Name     | ID         |
|---------------|------------|
| PWM Output    | 0x0\-0x7   |
| LCD           | 0x21       |
| Seven Segment | 0x20       |
| Timer         | 0x25\-0x28 |



## Program Examples
Check the folder named "Program Examples" 

