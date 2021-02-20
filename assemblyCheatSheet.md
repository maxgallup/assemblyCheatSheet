# X86 AT&T Syntax
### The Linker 
You can use .o .c .cpp files and link them together to compile one executable. 
```
as test.asm -o test.o
g++ text.o main.cpp -o runme 
```

### Data Sizes
* Byte: 8 bits
* Word: 2 bytes, `short int`
* Dword(Intel)/Long(AT&T): 4 bytes, `int`
* Qword/quad: 8 bytes, `long`

### General Syntax
Each sequence of instructions occupy one line.
```
Intel:
mov al, 23
mov bl, 6
add al, b1

AT&T:
movb $23, %al
movb $6, %bl
addb %bl, %al
```

### Source and Destination Operands
Intel vs AT&T source destination are swapped!

* AT&T can be read as "move 100 into eax"
* Intel can be read as "move into eax the value 100"


### Mnemonics
Human readable version of machine code. AT&T Syntax can have an extra letter which indicates that data size to operate on. If there is not suffix to an AT&T syntax, mnemonic the data size will be inferred from the destination operand.

* b -  byte (8)
* w - word (16)
* l - long (32)
* q - quad word (64)

---

### Registers 
Extremely fast storage directly on the CPU that allows for quick logic/arithmetic operations. Registers don't have an address, pointers only point to things in RAM. They don't have a data type (they are all data types at once / can be anything). Data isn't meant to reside in them for very long. Load data into them, make computations and then store data back into more permanent memory. X86 has general purpose registers, but also specialised registers. Size of registers is word length (32 vs 64 bit). Backwards compatibility work when allowing CPU to access subsets of registers. Processor can treat half of the register as if it was a 32-bit register.

### X86 General Register Set 
* AL/AH - two 8 bit wide registers, can be used separately 
* AX - combination of AH/AL = 16 bits
* EAX - 32 bits, top 16 bits can't be accessed directly, lower 16 bits are AX
* RAX - 64 bits, top 32 bits can't be accessed directly, combines all previous registers. 

16 RAX registers are available for general purpose(GPR general purpose registers).

#### 64 - Low32 - Low16 - Low8 - Notes
```
RAX		EAX     AX      AH/AL   Accumulator     
RBX		EBX     BX      BH/BL   Base    
RCX		ECX     CX      CH/CL   Counter 
RDX		EDX     DX      DH/DL   Data    
RSP		ESP     SP      SPL     Stack Pointer   
RBP		EBP     BP      BPL     Base Pointer    
RSI		ESI     SI      SIL     Source Index    
RDI		EDI     DI      DIL     Destination Index   
R7		R7D     R7W     R7B     Only on 64 bit  
R8      R8D     R8W     R8B     Only on 64 bit  
R9      R9D     R9W     R9B     Only on 64 bit  
R10     R10D    R10W    R10B    Only on 64 bit  
R11     R11D    R11W    R11B    Only on 64 bit  
R12     R12D    R12W    R12B    Only on 64 bit  
R13     R13D    R13W    R13B    Only on 64 bit  
R14     R14D    R14W    R14B    Only on 64 bit  
R15     R15D    R15W    R15B    Only on 64 bit  
``` 


### Special Registers
* `RIP` - Instruction Pointer, points to the spot in RAM where instructions are being read from
* `RFLAGS` - The flags register, maintains flags so we can check the conditions from recent instructions.
* `S[]` - The x87 floating point unit uses its own register set (largely legacy).
* `SIMD` - There's many different SIMD register sets which might be available depending on your CPU
* Machine Specific register handling specific tasks. 

---

### The Stack
Region of memory that acts as a Last in First Out data structure. Push and pop onto and from the stack. Implemented as contiguous array in memory. Stack Pointer holds location of the top of the Stack. Pushing means moving the stack pointer forwards and writing a value to that new location. Popping reads from the current pointer location and then moves the pointer backwards. Random Access to the memory. Stack pointer is also just a register that can be changed.

---

## Instructions AT&T syntax
* MOVx[src], [dest] - move/copy data
* ADDx[src], [dest] - addition
* SUBx[src], [dest] - subtraction
* INCx[src] - increment
* DECx[src] - decrement
* NEGx[src] - negate (swaps the sign +/-)
* IMULx[src], [dest] - signed multiply
* MULx[src], [dest] - unsigned multiply
* RET - return

*The suffix "x" is for then data type (see Mnemonics)[#mnemonics]. For intel syntax swap the order of src and dest and omit the "x" at the end.*

#### AT&T Example:
```
movw $15, %ax
addq %rdx, %rcx
subq $12, %rsp
incb %cl
decl %r9d
```

#### Intel example
```
mov ax, 15
add rcx, rdx
sub rsp, 12
inc cl
dec r9d
```

---

### C Calling Conventions
#### Passing Integer Values
First 6 integer variable are passed in the following registers, this includes bool, char, enum or anything.

1. RDI
2. RSI
3. RDX
4. RCX
5. R8
6. R9 (anything more than 6 will go to the stack) 

```
void SomeFunction(int a, char b, unsigned long c);
int a               EDI (32 bit version of RDI)
char b              SIL (8 bit version of RSI)
unsigned long c     RDX (it takes up all 64 bits)

```

#### Returning Integer Values
In order to return data, it automatically looks at RAX register:

* char/bool - AL
* short - AX
* int - EAX
* long - RAX

---
### Boolean Instructions

* AND - ANDx RDX, RAX
* OR - ORx RDX, RAX
* NOT - RAX (flips the bit)
* XOR - XOR RDX, RAX

CPU must always perform operations on 8, 16, 32 or 64 bits at once. Result gets stored in op2 for AT&T syntax
```
10011011
&&&&&&&&
10101100
=
10001000
```

---

### Shift Operations
SHL - shift left, multiplies by 2 to the power of X
SHR - shift right, divides by 2 to the power of X
SAR - Shift arithmetic right, signed divide


---

### Assembler Directives AT&T
* `.global` - (same as `global` in Intel)




---












