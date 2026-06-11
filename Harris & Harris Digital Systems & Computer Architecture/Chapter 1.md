# From Zero to One 

## Managing complexity
- Modern systems are made of billions of transistors, no single person can do this alone with equations. 
- Need to manage complexity without going too far in depth 

### Abstraction 
- Hide details that are not important 
- Each level of abstraction hides slightly more information from the previous 
- For example, Analog circuits abstract the physics and devices 
- Digital circuits restrict analog voltage to a '1' and '0'

### Discipline 
- Restrict design choices so we can work at a higher level of abstraction.

### Three Y's 
- Hierarchy 
	- divide a system into modules, then further subdivide until each piece is easy to understand 
- Modularity
	- Each module has well-defined function and interfaces so they can connect together easily without side effects. 
- Regularity
	- Seeks uniformity among the modules. Common modules can be reused. 

### Digital Abstraction 
- represent values with discrete value variables 
- Amount of information is calculated as $\log_{2}N$, where there are N distinct states 

## Number Systems
- Decimal, Hexadecimal, Octal, Binary etc. 

### Bytes, Nibbles, etc. 
- 8 bits is a byte 
- 4 bits is a nibble 
	- not used too often anymore
- Microprocessors deal with 'Words'
	- size of word depends on architecture and processor 
	- 64 bit most commonly used now. 

### Signed Binary Numbers
- A N-bit sign/magnitude number uses the MSB as the sign and N-1 bits as magnitude Max value of $2^{N-1}-1$

### Two's Complement numbers 
- Identical to unsigned binary numbers, exampt MSB aat position has weight of $-2^{N-1} -1$
	- To take the twos complement, do 1 - (each digit), then add 1. 
- when adding the N'th bits, the result (N+1th) bit is discarded 

## Logic Gates 
### Truth Tables

#### NOT
![[Pasted image 20260529172635.png]]

| A | Y |
|---|---|
| 0 | 1 |
| 1 | 0 |

#### Buffer
![[Pasted image 20260529172741.png]]

| A | Y |
|---|---|
| 0 | 0 |
| 1 | 1 |

#### AND
![[Pasted image 20260529172806.png]]

| A   | B   | Y   |
| --- | --- | --- |
| 0   | 0   | 0   |
| 0   | 1   | 0   |
| 1   | 0   | 0   |
| 1   | 1   | 1   |

#### OR
![[Pasted image 20260529172820.png]]

| A | B | Y |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

#### XOR
![[Pasted image 20260529172855.png]]

| A | B | Y |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### NAND
![[Pasted image 20260529172908.png]]

| A | B | Y |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### NOR
![[Pasted image 20260529172925.png]]

| A | B | Y |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

#### XNOR
![[Pasted image 20260529172953.png]]

| A | B | Y |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

## Beneath Digital Abstraction 
- must be able to tolerate noise as digital signals are still represented by continuous (analog) quantities 

### Supply voltage 
- ground (0V)
- High 3.3V/5V but now we have even lower to avoid excessive power delivery to resistors and to extend lifespan 
### Logic Levels 
- Mapping of continuous to digital binary is done by defining logic levels. 
- Usually we implement some sort of hysteresis (range in which low switches to high etc), 
- There are situations where we can be undefined... bad...

### Noise Margins 
- We choose the hysteresis range based on the noise figures we expect, such that the lowest high is still high and the highest low is still low. 

### Static Discipline 
- we do not switch until we reach a threshold. This way we can completely avoid any outputs that we deal with in the 'forbidden' zone. 

## CMOS Transistors 
- Consists of NMOS and PMOS transistors together. 

| Gate | NMOS | PMOS |
| ---- | ---- | ---- |
| 0    | 0    | 1    |
| 1    | 1    | 0    |

![[Pasted image 20260529191653.png]]

### Power Consumption: 
- energy used per unit time 
- Power consumption is of great importance to maximize battery life. 
- Digital systems have static and dynamic power. 
- Dynamic power is what happens between changing from 0 to 1 as charge capacitance changes. 
- For static it is just the leak current of the transistor. 
