# Digital Building Blocks
- more complex digital building blocks are built of simpler components such as mux/register, etc. 

## Arithmetic Circuits 
- Arithmetic circuits are central building blocks of computers 
	- preform addition/subtraction/comparison/shifts etc. 
### Addition 
- Half adder is the most basic adder circuit. 
- A full adder takes in a carry in bit. 
- To make a N bit adder, where N is a power of 2, chain together full adders to make a ripple adder, Cout goes into Cin of next bit. 

### Carry Lookahead Adder 
- Ripple carry adder is slow 
- carry propagate adder divides adder into blocks and provides circuitry to determine the carry out of a block as soon as the carry in is known. 
- Carry-lookahead uses generate and propagate signals that describe how a column or block determines carry out. 
- Ith block is said to generate a crry if it produces a caryr out independent of carry in. 
- I'th column of an adder is guaranteed to generate a carry in if A, B and are both one

### Prefix Adder 
- Extend the generate and propagate logic of carry lookahead adder to perform even faster addition. 
- The sum is created from the i-1th carry for each column. 

### Putting together 
- HDL languages provide the + operation and the synthesis tools choose the cheapest design to meet speed reqs. 


## Subtraction 
- can just takes the twos complement in hardware to do this. 

## Comparators 
- A comparator determines whether two binary numbers are equal or if one is greater or less than the other. 
	- Equality operator 
		- A == B 
		- uses XNOR
	- Magnitude comparator 
		- A > B
		- Preforms A-B and looks at the MSB of the result (sign bit)
### Arithmetic Logic Unit 
- Combines a variety of mathematical and logical operations into a single unit.
- Typical ALU may perform addition, subtraction, magnitude comparison, AND, OR operations. 
- ALU receives a control signal for which to perform 
- Sett less than: A < B, Y =1.
- Zero extended unit produces n bit output by concatenating its 1-bit input with 0's in MSB.
- Some ALUs produce flags such as overflow flag, zero flag etc. 

### Shifters and Rotators 
- used for multiplying or dividing by powers of two. 
- Two types of shifters 
	- Logical shifter 
		- Shifts the numbers to the left or right and fills the empty bit spots with a 0. 
		- Example 1101 ASL 2 = 00100
	- Arithmetic Shifters
		- Same as a logical shifter but on right shifts fills MSB with copy of old MSB. 
			- Useful for multiplying and dividing signed numbers
			- Example 1101 ASR 2 = 1110
			- 
- Rotators rotate numbers in circle such that empty spots are filled with bits shifted off the other end. 
	- Example ROL (rotate left)
		- 11001 ROL 2 = 00111
	- Example ROR (rotate right)
		- 11001 ROR 2 = 01110

- An N bit shifter can be built from N N:1 multiplexers 
	- Input can be shifted from 0 to N-1 bits 
	- for all shifters when shamt = 00, input = output. 
- A left shift by N bits is equivalent to multiplication by $2^N$


### Multiplication
- Multiplication of unsigned binary numbers is similar to decimal multiplication but involves only 1s and 0s. 
- Partial products are formed by multiplying a single digit of the multiplier with the entire operand 
- N X N = 2N 

``` verilog; 
module multiplier # (parameter N 8)
			(input [N 1:0] a, b,
			output [2*N 1:0] y);

	assign y a * b;
endmodule
```

### Division 
- Division is preformed with an algorithm 
- partial remainder etc. 
- Delay of N bit division is proportional to $N^2$ because array must ripple through all N stages in a row before sign is determined. 

## Number Systems 
- Computers operate on both integers and fractions. so far only consider representing signed or unsigned integers. 
- Fixed point numbers are like rational numbers, floating point numbers are like scientific notation 

### Fixed point Number System 
- Implies binary point between integer and fraction bits, analogous to the decimal point between the integer and fraction digits of an ordinary decimal number. 

### Floating Point Number System 
- Floating point numbers analogous to scientific notation. 
- Floating point numbers have sign, mantissa, base, and exponent. 
- in binary floating point, the first bit of the mantissa is always 1 and therefore need not be stored. 
	- implicit leading one 

### Special Cases 
- IEEE has special cases to represent numbers such as zero, infinity, and illegal results. 
- Representing 0 is problematic because of the implicit leading 1. 
- NaN does not exist 

### Single/Double Precision 
- 32 bit or 64 bit precision. 

### Rounding 
- Airhtmetic results that fall outside available precision must round to a neighboring number. 
- Round up, Round Down, Round towarad 0. 
- Overflow too large 
- Underflow too tiny


## Sequential Building Blocks

### Counters 
- An n bit counter is a sequential arithmetic circuit with clock and reset inputs and an N-bit output. 
- Reset initializes output to 0. 
	- Counter then goes through all $2^N$ outputs in binary order. 

example counter: 
``` verilog: 
module counter #(parameter N 8)
				(input clk,
				input reset,
				output reg [N 1:0] q);

always @ (posedge clk or posedge reset)
	if (reset) q <= 0;
	else q <= q 1;

endmodule
```

### Shift Registers 
- Shift register has a clock, a serial input, a serial output and N parallel outputs 
- On Each rising edge of clock a new bit is shifted from $S_{in}$ an all the subsequent bits are shifted forward 
- Shift registers can be viewed as serial-to-parallel converters
- Input is provided serially at $S_{in}$. After N cycles, the past N inputs are available in parallel at Q
- A related circuit is a parallel-to-serial converter that loads N bits in parallel then shifts them out one at a time. 
- A shift register can be modified to perform both Serial to Parallel or Parallel to Series 

### Scan Chains 
- Shift Registers are often used to test sequential circuits using a technique called scan chains. 
- Known inputs called test vectors are applied and the outputs are checked against the expected result
- Designers want to be able to directly observe and control all states of the machine, this is done by adding test mode in which contents of all flip flops can be read out or loaded with desired values. 

## Memory Arrays 
- Arithmetic and sequential circuits for manipulating data. 
- Digital systems also require memories to store the data used and generated by such circuits. 
- Registers built from flip flops are a kind of memory that store small amounts of data. 
- Memory arrays store large amounts of data. 


### Overview of memory 
- Memory is organized as 2 dimensional array 
- Row is where memory is written to. 
- Specified by an Address 
- Value read or written is Data 
- The depth of an array is the rows 
- The width is number of columns/word size 

### Bit Cells 
- Memory arrays are built as an array of bit cells, each of which stores 1 bit of data. 
- Each bit cell is connected to a wordline and a bitline. 
- To read a bitcell, the bitline is initially left floating (Z) then the wordline is turned on, allowing the stored value o drive the bitline to 0. 

### Organization 
- During am emory read, a wordline is asserted and the corresponding row of bit cells drives the bitlines high or low. 

###  Memory Types 
- Memory arrays are specified by their depth and width 
- All memory arrays store data as an array of bit cells 
- Memories are classified based on how they store bits in the bit cell 
- Broadest classification is random access memory versus read only memory 
- Ram is volatile meaning it loses data when power is turned off 
- ROM is non volatile meaning it retains data indefinitely 
- Two major types of ram are 
	- DRAM (dynamic)
	- SRAM (static)
- Dynamic RAM uses capacitors, static RAM stored data on cross-coupled inverters 

### Dynamic RAM 
- stores bit based on presence or absence of charge on capacitor 
- Bit value stoed across capacitor 
- nMos acts as a switch that either connects or disconnects transistor form bitline. 
- upon reading, data is transferred from capacitor to bitline. Upon a write, data values transferred from bitline to capactor. 

### Static Ram 
- Static Ram is static because stored bits do not need to refreshed 
- Each cell has two outputs, bitline and not bitline. 
- If the value degrades the cross coupled inverters store the value. 

### Area and Delay  
- Flip Flops, SRAM and DRAM are all volatile memories, but each has different areas and speeds. 
![[Pasted image 20260609000316.png]]

### Register Files 
- A group of registers, called a register file, has a small multiported SRAM Array. 

### Read Only Memory 
- stores a bit as the presence or absence of a transistor. 
- The contents of a RAM can be indicated using dot notation.
- A dot at the intersection of a row and a column indicates the the data bit is 1. 
- For example, the top wordline has a single dot on Data, so that data word stored at Address 11 is 010. 
- Conceptually, ROMs can be built using two-level logic with a goup of AND gates followed by a group of OR gates. 
- The AND gates produce all possible minterms and hence form a decoder. 

- A PROM, programmable ROM places a transistor in every bit cell but provides a way to connect or disconnect the transistor to ground. 
- A fuse programmable ROM, user can apply high voltage to selectively blow fuses, this is also called one-time programmable ROM. 
- Reprogrammable ROMs provide a reversible mechanism for connecting or disconnecting the transistor to ground. 
- Erasable PROMS replace the nMOS transistor and fuse with a floating-gate transistor. 
- Floating gate not physically attached. 
- EPROM and flash memory use similar principles but include circuitry on the chip for erasing as well as programming, so no UV light is necessary. 
- The difference between RAM and ROM is that ROM takes longer to write but nonvolatile. 

### Logic using memory arrays 
- Memory arrays can also perform combinational logic. 
- Memory arrays used to perform logic are called lookup tables. 
- Using memory to perform logic, the user can look up the output value for a given input combination. 
- Each address is a row in the truth table, each data bit corresponds to an output value. 
### Memory HDL 

example: RAM
``` verilog; 
module ram # (parameter N 6, M 32)
			(input clk,
			input we,
			input [N 1:0] adr,
			input [M 1:0] din,
			output [M 1:0] dout);

reg [M 1:0] mem [2**N 1:0];
	always @ (posedge clk)
if (we) mem [adr] din;
	assign dout mem[adr];

endmodule
```
- writes occur on the rising edge of the clock if write is enabled, we, is asserted. 
- Reads occur immediately
example: ROM
``` verilog;
	module rom (input [1:0] adr,
	output reg [2:0] dout);
always @ (adr)

case (adr)
	2b00: dout 3b011;
	2b01: dout 3b110;
	2b10: dout 3b100;
	2b11: dout 3b010;
endcase

endmodule
```

### Logic Arrays 
- Like memory, gates can be organized into regular arrays. 
- If the connections are made programmable, these logic arrays can be configured to perform any function without replacing hardware. 
- There are two types of logic arrays 
	- Programmable Logic Arrrays and FPGA
	- PLA is old 
- FPGA is an array of reconfigurable gates. Using software programming tools, can implement designs on FPGA using HDL or schematic. 
- FPGA more powerfula nd more fliexibel than PLA 
	- combinational + sequential 
- FPGA are built as array of configurable logic blocks 
- CLBs are surrounded by input/output blocks for interfacing with external devices 
- 