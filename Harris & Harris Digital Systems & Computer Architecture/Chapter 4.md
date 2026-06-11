# Hardware Description Languages
- so far we have designed combination and sequential circuits on schematic level 
- The process of finding simplifications takes long for larger state machines 
- To work at a higher level of abstraction, we use HDL (hardware description languages)
	- Verilog
	- System Verilog 
	- VHDL

## Modules 
- A block of hardware with inputs and outputs is a module 
- Two general styles ofr describing module functionality are 
	- Behavioural 
		- Describe what a model does
	- Structural 
		- How a module is built from simpler pieces 

- Verilog module begins with the module name and a listing of inputs and outputs. 
- Both languages are fully capable of describing any hardware system 

## Simulation and Synthesis 
- Simulation and Synthesis 
- The two major purposes of HDLs are logic simulation and synthesis. 
- During synthesis, the textual description of a module is transformed into logic gates. 

### Simulation 
- Logic simulation in crucial to test before a system is built 

## Synthesis 
- Logic synthesis transforms HDL code into a netlist describing the hardware. 
- Circuits in HDl resemble code in a programming language.
	- Verilog and VHDL have many commands, not all of these can be synthesized to hardware 
- The emphasis of HDL is a synthesizable susbet of languages 
- We divide HDL code into synthesizable modules and a testbench 
- we should think about RTL has hardware at its core. 

## Combinational Logic 
- sequential circuits are made of combinational blocks and registers 
- Outputs of combinational logic should only depend on current inputs 

### Bitwise Operators 

``` verilog; 
module inv (input [3:0] a,
			output [3:0] y);
			
		assign y=~a
endmodule
```
Inside the square brackets is a 4-bit bus. 
This is little endian. 

``` verilog;
module gates(input [3:0] a, b,
		output [3:0] y1, y2,
	
y3, y4, y5);

	/* Five different two-input logic
	
	gates acting on 4 bit busses */
	assign y1 a & b; // AND
	assign y2 a | b; // OR
	assign y3 a b; // XOR
	assign y4 ~(a & b); // NAND
	assign y5 ~(a | b); // NOR

endmodule
```

All of the above are verilog operators, a,b,y1 are called operands. 

- assign out = in1 op in2; 
	- continuous assignment statement 
	- any time inputs on right side of the = change in a continuous assignment change, outputs on the left side is recomputed. 

## Reduction Operators 
- Reduction operators imply a multiple-input gate acting on a single bus. 
- ex: 
	- y = &a 
		- ands every bus of A with each other 

## Conditional Assignment 
- The conditional operator ?: chooses based on a first expression whether to choose the first operation or the second operation 

Example: multiplexer 

``` verilog: 
module mux2(input [3:0] d0, d1,
			input            s,
			output [3:0] y   ); 
		
		assign y = s? d1 : d0
		
endmodule
```

- If s is 1, then y = d1. If s is 0, then y = d0 
- ?: is also called a ternary operator, because it takes three inputs.

Example: 4 to 1 Mux

``` verilog; 
module mux4(input [3:0] d0, d1, d2, d3,
			input [1:0] s,
			output [3:0] y);

assign y s[1] ? (s[0] ? d3 : d2) 
			: (s[0] ? d1 : d0);

endmodule
```

### Internal Variables 
- often convenient to break a complex function into intermediate steps 
- We can define intermediate signals 
	- They are called internal variables, because they are neither inputs nor outputs but are internal to the module 
- HDL assignment statements (assign in verilog) take place concurrently
- Different from conventional programming languages such as C in which statements are evaluated in the order they are written. 

### Example: Full adder 
- In Verilog, wires are used to represent internal variables. 
- Values are defined by assign statements such as: 
	- assign p = a ^ b
- Wires technically only need to be declared only for multibit busses, but it is good to declare them for all internal variables. 

``` verilog;
module fulladder(input a, b, cin,
				output s, cout);

	wire p, g;
	
	assign p a b;
	assign g a & b;
	
	assign s p cin;
	assign cout g | (p & cin);

endmodule
```

- In HDL it does not matter which operation comes first, because statements are evaluated at any time the inputs, signals on the RHS change their vlsue, regardless of order of assignment. 

## Precedence 
- Notice that we parenthesized the cout computation in HDL to define the order of operations. 
- if we do not use parentheses, default operation order is defined by language. 

### Numbers 
- Numbers can be specified in a variety of bases. 
- Underscores can be ignored 
- Verilog numbers
![[Pasted image 20260605185635.png]]

- HDLs also use X to indicate an invalid logic level. 
- If a bus is simultaneously driven to 0 and 1 by two enabled tristate buffers, the result is x, indicating contention 
- At the start of simulation, all state nodes such as flip flop outputs are initialized to an unknown state (x in verilog)
	- this is helpful to track errors such as forgetting to reset a flip flop 
- If a gat receives a floating input, it may produce an x when it can't determine the correct output value. 
- If it receives an illegal or uninitallized input, it may produce an x output
- seeing an x or u value in simulation is almost always a bug or bad coding practice.

### Bit Swizzling 
- Often necesasry to operate on a subset of a bus or to concatenate signals to form busses. 
- These operatings are called bit swizzling
``` verilog; 
assign y {c[2:1], {3{d[0]}}, c[0], 3’b101};
```
{} is used to concatenate buses. 

### Delays 
- HDL statements are associated with delays 
	- These are specified in arbitrary units 
- These delays are ignored during synthesis
	- delay of a gate produced by the synthesizer depends on its Tpd and Tcd specifications, not on numbers in HDL code. 
	- HDL code assumes that some functions have a delay of x nanoseconds 

## Structural modelling 
- structural modelling, describes module composed of simpler modules 

example: 2 to 1 mux: 
``` verilog; 
module mux4(input [3:0] d0, d1, d2, d3,
					input [1:0] s,
					output [3:0] y);

wire [3:0] low, high;

mux2 lowmux(d0, d1, s[0], low);
mux2 highmux(d2, d3, s[0], high);
mux2 finalmux(low, high, s[1], y);

endmodule
```

- each copy of a mux here is called an instance. 
- multiple instances of the same module are distinguished by distinct names. 
- In this case lowmux, highmux, and finalmux. 

``` verilog; 
module mux2(input [3:0] d0, d1,
				input s,
				output [3:0] y);
	
	tristate t0(d0, ~s, y);
	tristate t1(d1, s, y);
endmodule
```

- In general complex systems are designed hierarchically. 
- The overall system is described structurally by instantiating its major components 
- best practice to avoid mixing structural and behavioural in the same module 

Example: Accessing parts of busses 
``` verilog;
module mux2_8(input [7:0] d0, d1,
				input s,
				output [7:0] y);

mux2 lsbmux(d0[3:0], d1[3:0], s, y[3:0]);
mux2 msbmux(d0[7:4], d1[7:4], s, y[7:4]);

endmodule
```


## Sequential logic 
- HDL synthesizers can recognize certain idioms and turn them into specific sequential circuit. 


### Registers 
- vast majority of modern commercial systems are built with registers using positive edge triggered D flip flops 
- In Verilog, always statements signals keep their old value until an event in the sensitivity list take place that explicitly makes them change 
- Example, flip flop only includes clk in the sensitive list. It remembers its old value of q until the next rising edge of the clk, even if d changes in the interim 
- Verilog continuous assignments are re-evaluate anytime any of the inputs on the rhs change 

register example:
``` verilog; 
module flop(input clk,
		input [3:0] d,
	output reg [3:0] q);

always @ (posedge clk)
q d;

endmodule
```
- Verilog always statement always written in the form 
	- always @ (sensitivity list)
- q,d is q gets d.

### Resettable Registers 
- When a simulation begins or power is first applied to a circuit, output of flop or register is unknown 
- Generally it is good practice to use resettable registers so we can put register in a known state 
	-  reset can be applied synchronously or asynchronously 
		- asynchronous immediately 
		- synchronous on next clock edge 

``` verilog; 
module flopr(input clk,
			input reset,
			input [3:0] d,
		output reg [3:0] q);

// asynchronous reset

	always @ (posedge clk, posedge reset)
	
	if(reset) q 4’b0;
	else q d;

endmodule

module flopr (input clk,

	input reset,
	input [3:0] d,
	output reg [3:0] q);

// synchronous reset

	always @ (posedge clk)
	if (reset)q 4’b0;
	else q d;

endmodule
```

- Multiple signals in an always statement sensitivity list are separated by commas or the word or. 

### Enabled Registers 
- Enabled registers respond to the cock only when the enable is asserted 

``` verilog; 
module flopenr(input clk,
			input reset,
			input en,
		input [3:0] d,
	output reg [3:0] q);

// asynchronous reset

always @ (posedge clk, posedge reset)

if(reset)q 4’b0;
else if(en)q d;

endmodule
```

### Multiple Registers 
- a single always statement can be used to describe multiple pieces of hardware 

``` verilog; 
module sync(input clk,
			input d,
			output reg q);

reg n1;
always @ (posedge clk)

begin
	n1 d;
	q n1;
end

endmodule
```
- begin end are used to execute multiple statements in a sequential block, executed in the exact order they are listed 

### Latches 
- A d-latch is transparent when the clock is high 
	- data can flow from input to output. 
- Latch becomes opaque when clock is low, retaining its old state. 
- Not all synthesis tools support latches well. 

ex. d latch: 
``` verilog: 
module latch(input clk,
		input [3:0] d,
		output reg [3:0] q);

	always @ (clk, d)
	if (clk) q d;

endmodule
```

### More Combinational Logic 
- Verilog always statements are used to describe sequential circuits, because they remember the olds state when no new state is prescribed. 
- Always statements can be used to describe combinational logic behaviourally. 
- always statements can also be used to describe combinational logic behaviourally.
	- if the sensitivity list is written to respond changes in all of the inputs.

Example: inverter using always
``` verilog; 
module inv(input [3:0] a,
		output reg [3:0] y);
		always @ (*)

		y ~a;

endmodule
```

- sensitivity list is written to respond to changes in all of the inputs and the body prescribes the output value for every possible input combination 

- HDLs support blocking and noblocking assignments in always statements 
- A group of blocking assignments are evaluated in the exact order they appear in the code 

``` verilog; 
module fulladder(input a, b, cin,
			output reg s, cout);
	reg p, g;
	always @ (*)

	begin
		p a b; // blocking
		g a & b; // blocking
		s p cin; // blocking
		cout g | (p & cin); // blocking
	end

endmodule
```

### Case Statements 
- a better application of using the always statement for combinational logic is a 7-seg display decoder that takes advantage of case statement that must appear inside always statement 

``` verilog; 
module sevenseg(input [3:0] data,
		output reg [6:0] segments);
		always @ (*)

	case(data)

// abc_defg
	
		0: segments 7’b111_1110;
		1: segments 7’b011_0000;
		2: segments 7’b110_1101;
		3: segments 7’b111_1001;
		4: segments 7’b011_0011;
		5: segments 7’b101_1011;
		6: segments 7’b101_1111;
		7: segments 7’b111_0000;
		8: segments 7’b111_1111;
		9: segments 7’b111_1011;
		
		default: segments 7’b000_0000;

	endcase

endmodule
```
- default clause is convenient to define output for all cases that are not explicitly listed. 
- case statement performs different actions depending on the value of its input 
- Case statement implies combinational logic if all possible input combinations are defined 
	- otherwise it is sequential because output keeps old value in certain undefined cases 

### If Statements 
- Always statements may also contain if statements. 
- The if statement also can have else 

``` verilog; 
module priority(input [3:0] a,
			output reg [3:0] y);
	always @ (*)

	if (a[3]) y 4’b1000;
	else if (a[2]) y 4’b0100;
	else if (a[1]) y 4’b0010;
	else if (a[0]) y 4’b0001;
	else y 4’b0000;
endmodule
```
- If all input combinations are handled it implies combinational logic 
	- otherwise is sequential logic 
- Priority circuit sets the output true that corresponds to the most significant input that it true. 

### Verilog Casez 
- Verilog also provides the casez statement to describe truth tables that have dont cares. 

example: 
``` verilog; 
module priority_casez(input [3:0] a,
				output reg [3:0] y);

	always @ (*)

	casez (a)
		4’b1???: y 4’b1000;
		4’b01??: y 4’b0100;
		4’b001?: y 4’b0010;
		4’b0001: y 4’b0001;
		default: y 4’b0000;
	endcase

endmodule
```
### Blocking and Nonblocking Assignments
- If you do not follow proper guidelines the simulation may work but may synthesize to incorrect hardware.

**Guidelines**
``` verilog; 
1. Use always @ (posedge clk)and nonblocking assignments

to model synchronous sequential logic.

	always @ (posedge clk)
	begin
		nl d; // nonblocking
		q nl; // nonblocking
	end

2. Use continuous assignments to model simple combinational logic.

assign y s ? d1 : d0;

3. Use always @ (*) and blocking assignments to model more complicated combinational logic where the always statement is helpful.

always @ (*)
begin
	p a b; // blocking
	g a & b; // blocking
	s p cin;
	cout g | (p & cin);
end

4. Do not make assignments to the same signal in more than one always statement or continuous assignment statement.
```

- block assignments follow the following order: 
![[Pasted image 20260607103022.png]]

### Combinational 
``` verilog; 
// nonblocking assignments(not recommended)

module fulladder (input a, b, cin,
			output reg s, cout);

	reg p, g;
	always @ (*)
	
	begin
		p a b; // nonblocking
		g a & b; // nonblocking
		s p cin;
		cout g | (p & cin);
	end
endmodule
```
- Another drawback of nonblocking assignments in modelling combination logic is that the HDL will produce the wrong result if you forget to include intermediate variables in sensitivity list. 

### Sequential Logic 
- the synchronizer is correctly modelled using non blocking assignments.
- Always use nonblocking assignment in always statements when modelling sequential logic. 

## Finite State Machines
- We recall that a finite state machine consists of a state register and two blocks of combinational logic to compute the next state and the output of the machine 

Example: Divide by three state machine 

``` verilog; 
module divideby3FSM(input clk,
				input reset,
				output y);

reg [1:0] state, nextstate;

parameter S0 2b00;
parameter S1 2b01;
parameter S2 2b10;

// state register

always @ (posedge clk, posedge reset)
	if (reset) state S0;
	else state nextstate;


always @ (*)
	case (state)

	S0: nextstate S1;
	S1: nextstate S2;
	S2: nextstate S0;

	default: nextstate S0;

endcase
// output logic
assign y (state S0);
endmodule
```
- the parameter statement is used to define constants within module 
- A case statement can be used to define the state transition table. 
- Next state logic should be combinational, a default is necessary even though state should never arise.
- The output, y, is a 1 when the state is S0. The equality comparison evaluates to 1 a == b
- The inequality comparison a != b does the inverse 

 ``` verilog; 
module patternMoore(input clk,
				input reset,
				input a,
				output y);

reg [2:0] state, nextstate;
	
	parameter S0 3b000;
	parameter S1 3b001;
	parameter S2 3b010;
	parameter S3 3b011;
	parameter S4 3b100;

// state register

always @ (posedge clk, posedge reset)
	if (reset) state S0;
	else state nextstate;

// next state logic

always @ (*)

case(state)
	S0: 
		if (a) nextstate S1;
		else nextstate S0;

	S1: 
		if (a) nextstate S2;
		else nextstate S0;

	S2: 
		if (a) nextstate S2;
		else nextstate S3;
	
	S3: 
		if (a) nextstate S4;
		else nextstate S0;

	S4: 
		if (a) nextstate S2;
		else nextstate S0;

	default: nextstate S0;

endcase

	// output logic
	
	assign y (state S4);

endmodule
 ```
 - non blocking used for state registers for sequential logics  (<=)
 - blocking assignments used for next state logic for combinational logic 

### Testbenches 
- A testbench is an HDL module that is used to test another module 
	- called device under test 
- The input and desired output patterns are called test vectors 
- Testbenches are simulated the same as HDL modules but not synthesizeable 

example: 

``` verilog; 
module testbench1();
	reg a, b, c;
	wire y;

// instantiate device under test

	sillyfunction dut(a, b, c, y);

// apply inputs one at a time

	initial begin
		a 0; b 0; c 0; #10;
		c 1; #10;
		b 1; c 0; #10;
		c 1; #10;
		a 1; b 0; c 0; #10;
		c 1; #10;
		b 1; c 0; #10;
		c 1; #10;
	end

endmodule
```
- initial statement executes the statements in its body at the start of simulation. 
- This case, it first applies the pattern 000 and waits for 10 time units. 
- Initial statements should only be used in testbenches for simulation
- checking for correct outputs is tedious and error-prone 
- write self checking testbenches

- best practice is to place test vectors in a separate file. 
- The testbench reads the vectors from the files and checks against a DUT. 
- Repeats until end of test vector file. 
## Questions 
1. 
``` verilog; 
assign result = sel ? data : 32'b0
```
- sel = 1, pass data into result 
- sel = 0 make result all 0s. 

2. Explain blocking vs nonblocking assignments 
- blocking has = , happens immediately line by line. 
- Nonblocking has <=, happens at the same time (end of time step) LHS all update together. 
- combinational logic use blocking, sequential logic use non blocking