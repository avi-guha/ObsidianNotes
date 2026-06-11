- microarchitecture is the bridge between logic and architecture 
- specific arrangement of registers, ALUs, FSMs, Memories etc. 
- MIPS processor - microprocessor without interlocked pipelined stages 

### Design process  
- Divide the microarchitecture into two interacting parts: 
	- datapath 
		- operates on words of data 
		- contains structures including memory, registers, alu, mux
		- N bit structure has N bit data path
	- control 
		- receives current instruction and tells data path how to execute 
		- produces mux select, register enable, and memory write signals 
- A good way to design a complex system is to start with hardware containing state elements 
	- contains memories and architectural state 
		- PC and registers 
		- Add blocks of combinational logic between state elements to compute new state based on current state
	- convenient to partition overall memory into smaller memories 
		- instructions and data 
- Program counter is a regular 32 bit register, its output, PC points to the current instruction. 
	- Input PC' points to address of next instruction 
![[Pasted image 20260609102105.png]]

- instruction memory has a single read port. 
	- Takes 32 bit instruction from A, reads the 32 bit data from that address 
![[Pasted image 20260609132738.png]]

- 32 element x 32 bit register file has two read ports and one write port 
	- Read port takes 5 bit address port, specifying one of the 32 registers as source operands (RD1 and RD2)
	- Write port takes a 5 bit address input and a 32 bit write data output 
	- If write data is enabled and a clock rises, the register file writes data into specified register on rising edge of clock 
![[Pasted image 20260609132932.png]]
- Data memory has a single read/write port. If the write enable is 1, it writes data into an address A on the rising edge of clock. If write enable is 0 it reads address A onto RD

![[Pasted image 20260609133015.png]]

- The instruction memory, register file, and data memory are all read combinationally. 
- If address changes, new data appears at RD after some propagation delay; with no clock. 
- Because state elements only change on rising edge of clock, they are synchronous sequential circuits. 
- A processor can be viewed as a giant fsm or a collection of interacting FSM

### MIPS Microarchitectures 
- There are three microarchitectures discussed 
	- Single-cycle 
		- executes an entire instruction in one cycle 
		- simple to explain and simple control unit 
		- In one state does not require any non-architectural state 
	- Multi-cycle 
		- executes instructions in a series of shorter ones 
		- simpler instructions in fewer cycles than complicated ones 
		- reduces hardware cost by reusing hardware 
		- this is accomplished by adding several non-architectural registers to hold intermediate results
		- multi-cycle processor only executes one instruction at a time, but each instruction takes multiple cycles. 
	- Pipelined
		- applies pipelining to the single cycle microarchitecture 
		- can execute several instructions simultaneously, improving throughput 
		- pipelining must add logic to handle dependencies between simultaneously executing instructions 
		- requires non-architectural pipelining registers 
		- all commercial processors are pipelined. 

## Performance Analysis 
- A particular processor architecture can have many microarchitectures with different cost and performance trade offset
- Processors are compared against each other using benchmarks 


## Single Cycle Processor 
- A single cycle microarchitecture begins with datapath that connects state elements with combinational logic 

### Single Cycle Datapath 
- The program counter register contains address of instruction to execute 
- First step is to read. 
- The instruction memory reads out or fetches the 32 bit instruction 
- ![[Pasted image 20260609200257.png]]
- Processor's actions depend on the specific instruction that was fetched. 
- For a lw instruction, the next step is to read the source register containing the base address. 
- The register is specified in the rs field of the instruction 
![[Pasted image 20260609200952.png]]
- the processor must add base address to the offset to find the address to read from memory. 
- The ALU receives two operands, SrcA and SrcB 
	- SrcA from register file 
	- SrcB from sign-extended immediate 
- The three bit ALUControl signal specifies the operation, which in turn generates the ALUResult and a zeroflag 
	- for a lw instruction, ALUControl signal should be set to 010 to add base address and offset .
![[Pasted image 20260609201147.png]]
- The destination for the lw instruction is specified in the rt field of the instruction, which is connected to port 3 of the address input of the register file. 
- While the instruction is being executed, the processor must compute the address of the next instruction. 
- Because instructions are 4 bytes each, the next instruction is at PC + 4
- Next we can extend the datapath to handle sw instruction. 
	- sw reads from base address 
	- ALU adds base address to immediate to find memory address 

![[Pasted image 20260609201343.png]]
![[Pasted image 20260609201357.png]]

![[Pasted image 20260609202403.png]]

### Single-Cycle Control:
- The control unit computes the control signals based on the opcode and funct fields of instructions 
- R-type instructions use the funct field to determine ALU operation 
- The main decoder computes most of the outputs from the opcode. 
- Also determines a 2-bit ALUOp signal. 
- ![[Pasted image 20260609202550.png]]
![[Pasted image 20260609202947.png]]

![[Pasted image 20260609205157.png]]

## Multicycle Processor 
- multicycle processor address weaknesses in single cycle process 
	- requires long enough clock cycle for slowest instruction 
	- Requires three adders 
	- And third, it has separate instruction and data memories, which may not be realistic. 
- multicycle addresses these weaknesses by breaking an instruction into multiple shorter steps 
- We design a multicycle processor following the same procedure we used for single-cycle processor. 
- First construct data path by connecting architectural state elements and memories with combinational logic. 

### Multicycle datapath
- A multicycle datapath reuses the same hardware over several cycles. 
	- one memory can hold instructions and data 
	- one ALU can compute PC + 4, branch targets, addresses, and arithmetic results 
	- extra registers hold intermediate values between cycles 
- Non-architectural registers are visible only inside the processor. 
	- IR = instruction register 
	- MDR = memory data register 
	- A and B = register file read values 
	- ALUOut = saved ALU result 
- These registers are needed because an instruction no longer completes all work in one long cycle. 
- Common multicycle steps:
	- Fetch: read instruction and compute PC + 4
	- Decode: read registers and compute possible branch target
	- Execute: perform ALU operation or address calculation
	- Memory: read or write data memory
	- Writeback: write result to register file
- Different instructions use different numbers of cycles. 
	- R-type usually uses fetch, decode, execute, writeback
	- lw uses fetch, decode, address calculation, memory read, writeback
	- sw uses fetch, decode, address calculation, memory write
	- beq uses fetch, decode, compare/branch

Example cycle breakdown:

| instruction | cycles | main work |
|---|---:|---|
| R-type | 4 | fetch, decode, execute, write register |
| lw | 5 | fetch, decode, address, read memory, write register |
| sw | 4 | fetch, decode, address, write memory |
| beq | 3 | fetch, decode, compare and update PC |

- Main advantage: shorter clock period than single-cycle because each cycle does less work. 
- Main disadvantage: CPI is greater than 1 because each instruction takes multiple cycles. 

### MultiCycle Control 
- Multicycle control is usually an FSM. 
- The current state says which step of the instruction is being performed. 
- The opcode and funct fields decide which state comes next. 
- Control signals are asserted only when needed in that cycle. 
	- IRWrite enables the instruction register during fetch 
	- PCWrite updates PC unconditionally 
	- PCWriteCond updates PC only when a branch condition is true 
	- RegWrite updates the register file 
	- MemWrite updates data memory 
	- IorD chooses whether memory address comes from PC or ALUOut 
- The ALU input muxes are more flexible than in single-cycle because the same ALU is reused. 
	- ALUSrcA chooses PC or register A
	- ALUSrcB chooses B, 4, sign-extended immediate, or shifted immediate

Example FSM idea:

``` verilog;
always @ (*)
	case (state)
		FETCH:  nextstate = DECODE;
		DECODE: case (op)
			6'b000000: nextstate = RTYPEEX;
			6'b100011: nextstate = MEMADR;  // lw
			6'b101011: nextstate = MEMADR;  // sw
			6'b000100: nextstate = BEQEX;
			default:   nextstate = FETCH;
		endcase
		MEMADR:  nextstate = (op == 6'b100011) ? MEMRD : MEMWR;
		MEMRD:   nextstate = MEMWB;
		MEMWB:   nextstate = FETCH;
		MEMWR:   nextstate = FETCH;
		RTYPEEX: nextstate = RTYPEWB;
		RTYPEWB: nextstate = FETCH;
		BEQEX:   nextstate = FETCH;
	endcase
```

- The FSM output logic produces the datapath control signals for each state. 
- Multicycle control is more complex than single-cycle control, but hardware cost is lower. 


## Pipelined Processor 
- Pipelining improves throughput by overlapping multiple instructions. 
- It does not make one instruction have less latency; it lets many instructions be in progress at once. 
- Ideal pipelined CPI approaches 1 after the pipeline fills. 
- Clock period is set by the slowest pipeline stage plus pipeline register overhead. 

Example:
- Laundry analogy for instructions:
	- single-cycle: finish one complete load before starting the next
	- pipelined: wash one load while drying another and folding another

### Pipelined Datapath 
- MIPS instructions are commonly split into five pipeline stages:
	- IF = instruction fetch
	- ID = instruction decode and register read
	- EX = execute or address calculation
	- MEM = data memory access
	- WB = register writeback
- Pipeline registers separate the stages. 
	- IF/ID holds fetched instruction and PC + 4
	- ID/EX holds decoded control signals, register values, and immediates
	- EX/MEM holds ALU result, write data, and memory control
	- MEM/WB holds memory data or ALU result for writeback
- Control signals move through the pipeline with the instruction that needs them. 
- Each stage works on a different instruction in the same clock cycle. 

Example pipeline timing:

| cycle | instr 1 | instr 2 | instr 3 | instr 4 |
|---:|---|---|---|---|
| 1 | IF |  |  |  |
| 2 | ID | IF |  |  |
| 3 | EX | ID | IF |  |
| 4 | MEM | EX | ID | IF |
| 5 | WB | MEM | EX | ID |

Example pipeline register:

``` verilog;
module flopenr #(parameter WIDTH = 8)
	(input clk,
	 input reset,
	 input en,
	 input [WIDTH-1:0] d,
	 output reg [WIDTH-1:0] q);

	always @ (posedge clk, posedge reset)
		if (reset) q <= 0;
		else if (en) q <= d;
endmodule
```

### Pipelined control
- Pipelined control starts like single-cycle control, then stores control signals in pipeline registers. 
- Control signals are grouped by the stage where they are used. 
	- EX controls: RegDst, ALUSrc, ALUControl
	- MEM controls: Branch, MemWrite
	- WB controls: MemtoReg, RegWrite
- If an instruction is stalled or flushed, control signals can be forced to 0 to create a nop. 
- A nop does not change architectural state because it writes no register and writes no memory. 

Example nop insertion:

``` verilog;
assign controlsD = stallD ? 9'b0 : decodedControls;
```

- In practice, hazard logic decides when to stall, flush, or forward values. 

### Hazards 
- A hazard is a situation that prevents the next instruction from executing in its normal cycle. 
- Three main types:
	- structural hazard: two stages need the same hardware at the same time
	- data hazard: instruction needs a result that has not reached writeback yet
	- control hazard: processor does not yet know the next PC after a branch or jump

### Structural Hazards
- A structural hazard happens when hardware resources are insufficient. 
- Example: one memory used for both instruction fetch and data access. 
	- IF wants to read an instruction
	- MEM wants to read/write data
- Fixes:
	- stall one instruction
	- duplicate the resource, such as separate instruction and data memories

### Data Hazards
- RAW, read after write, is the important hazard in the simple MIPS pipeline. 
- Example:

``` mips;
add $s0, $s1, $s2
sub $t0, $s0, $s3
```

- sub needs $s0 before add has written it back. 
- Forwarding, also called bypassing, sends a result directly from a later pipeline stage to an earlier stage. 
- Forwarding avoids many stalls. 
- A load-use hazard still usually needs a stall because memory data is not ready until the end of MEM. 

Example load-use hazard:

``` mips;
lw  $s0, 0($sp)
add $t0, $s0, $s1
```

- The processor stalls one cycle, then forwards the loaded value. 

Example forwarding choice:

``` verilog;
always @ (*)
begin
	ForwardAE = 2'b00;
	if (RegWriteM && (WriteRegM != 0) && (WriteRegM == RsE))
		ForwardAE = 2'b10;
	else if (RegWriteW && (WriteRegW != 0) && (WriteRegW == RsE))
		ForwardAE = 2'b01;
end
```

Example load-use stall:

``` verilog;
assign lwstall = MemtoRegE && ((RtE == RsD) || (RtE == RtD));
assign stallF = lwstall;
assign stallD = lwstall;
assign flushE = lwstall;
```

### Control Hazards
- Branches cause control hazards because the processor does not immediately know which instruction comes next. 
- Simple fixes:
	- stall until branch decision is known
	- predict branch not taken and flush wrong instructions if branch is taken
	- move branch comparison earlier in the pipeline
- Flushing changes instructions in earlier stages into nops. 

Example branch flush:

``` verilog;
assign pcsrcD = BranchD & EqualD;
assign flushD = pcsrcD;
```

- A delayed branch is another strategy where the instruction after the branch always executes. 
- Modern processors usually use branch prediction instead. 

### HDL Representation  of single-cycle processor 
- A processor is described hierarchically in HDL. 
- The top-level module connects the controller and datapath. 
- The controller decodes opcode and funct fields. 
- The datapath contains the PC, register file, ALU, memories, muxes, and sign extension. 

Example top-level structure:

``` verilog;
module mips(input clk,
		input reset,
		output [31:0] pc,
		input [31:0] instr,
		output memwrite,
		output [31:0] aluout,
		output [31:0] writedata,
		input [31:0] readdata);

	wire memtoreg, alusrc, regdst, regwrite, jump, pcsrc, zero;
	wire [2:0] alucontrol;

	controller c(instr[31:26], instr[5:0], zero,
		     memtoreg, memwrite, pcsrc,
		     alusrc, regdst, regwrite, jump,
		     alucontrol);

	datapath dp(clk, reset, memtoreg, pcsrc,
		    alusrc, regdst, regwrite, jump,
		    alucontrol, zero, pc, instr,
		    aluout, writedata, readdata);
endmodule
```

Example main decoder:

``` verilog;
always @ (*)
	case (op)
		6'b000000: controls = 9'b110000010; // R-type
		6'b100011: controls = 9'b101001000; // lw
		6'b101011: controls = 9'b001010000; // sw
		6'b000100: controls = 9'b000100001; // beq
		6'b001000: controls = 9'b101000000; // addi
		6'b000010: controls = 9'b000000100; // jump
		default:   controls = 9'bxxxxxxxxx;
	endcase
```

- The exact bit order of controls depends on how the local design packs the control signals. 
- The important idea is that each instruction maps to the control signals needed by the datapath. 

Example ALU decoder:

``` verilog;
always @ (*)
	case (aluop)
		2'b00: alucontrol = 3'b010; // add
		2'b01: alucontrol = 3'b110; // subtract
		default: case (funct)
			6'b100000: alucontrol = 3'b010; // add
			6'b100010: alucontrol = 3'b110; // sub
			6'b100100: alucontrol = 3'b000; // and
			6'b100101: alucontrol = 3'b001; // or
			6'b101010: alucontrol = 3'b111; // slt
			default:   alucontrol = 3'bxxx;
		endcase
	endcase
```


## Advanced Microarchitecture 
- Advanced microarchitectures improve performance by reducing stalls, increasing clock frequency, or executing more instructions per cycle. 
- These techniques preserve the same architecture, so the programmer sees the same ISA. 

### Deep Pipelines
- A deep pipeline has more stages than the basic five-stage pipeline. 
- Splitting work into more stages can reduce clock period. 
- More stages also increase branch penalty and pipeline register overhead. 
- Deep pipelines are useful only if the shorter clock period outweighs extra hazards and overhead. 

### Branch Prediction
- Branch prediction guesses the next PC before the branch is resolved. 
- Good prediction keeps the pipeline full. 
- Bad prediction requires flushing wrong-path instructions. 
- Static prediction uses a fixed rule, such as predict not taken. 
- Dynamic prediction uses past branch behavior. 
	- 1-bit predictor remembers last outcome
	- 2-bit predictor changes direction only after two wrong outcomes
	- branch target buffer stores predicted target addresses

Example 2-bit predictor states:

| state | prediction | taken result | not taken result |
|---|---|---|---|
| strongly taken | taken | stay | weakly taken |
| weakly taken | taken | strongly taken | weakly not taken |
| weakly not taken | not taken | weakly taken | strongly not taken |
| strongly not taken | not taken | weakly not taken | stay |

### Superscalar processor 
- A superscalar processor can issue multiple instructions in one cycle. 
- It needs multiple functional units and logic to find independent instructions. 
- Example: issue one ALU instruction and one memory instruction in the same cycle. 
- Superscalar improves IPC, instructions per cycle, but control and hazard logic become much more complex. 

### Out of order Processor 
- Out-of-order processors execute instructions when operands and functional units are ready, not strictly in program order. 
- This hides stalls from cache misses and long-latency operations. 
- The processor must still appear to complete instructions in program order. 
- A reorder buffer helps commit results in the original program order. 

### Register Renaming 
- Register renaming removes false dependencies caused by reusing architectural register names. 
- WAR, write after read, and WAW, write after write, are false dependencies. 
- The processor maps architectural registers to a larger set of physical registers. 

Example:

``` mips;
add $t0, $t1, $t2
sub $t0, $s1, $s2
```

- Both write $t0, but the writes are independent if later instructions use the correct version. 
- Renaming gives each write a different physical destination register. 

### Multithreading 
- Multithreading keeps multiple threads active on one processor core. 
- When one thread stalls, another thread can use the pipeline. 
- Fine-grained multithreading switches threads every cycle. 
- Simultaneous multithreading, SMT, can issue instructions from multiple threads in the same cycle. 
- SMT is often called hyper-threading in commercial processors. 

### Multiprocessor 
- A multiprocessor contains multiple processor cores. 
- Cores may share memory, caches, and interconnects. 
- Multiprocessors improve throughput by running different threads or programs in parallel. 
- Main challenges:
	- cache coherence, keeping shared data consistent
	- synchronization, coordinating threads
	- memory consistency, defining when writes become visible to other cores
- Parallel speedup is limited by the part of the program that cannot be parallelized. 

### Performance Summary
- Performance depends on instruction count, CPI, and clock period. 

$$Execution\ Time = Instruction\ Count \times CPI \times Clock\ Period$$

- Single-cycle has CPI = 1 but long clock period. 
- Multicycle has shorter clock period but CPI greater than 1. 
- Pipelined ideally has CPI near 1 and shorter clock period. 
- Real processors lose performance from hazards, cache misses, branch mispredictions, and pipeline overhead. 
