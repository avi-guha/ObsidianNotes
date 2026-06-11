# Combinational Logic Design 
- we can treat circuits as a black box 
	- one or more input terminals 
	- one or more output terminals 
	- a functional description of what the circuit does 
- A combinational circuit is memoryless 
	- sequential circuits have memory
- A circuit is combinational if: 
	- every circuit element is combinational 
	- Every node of the circuit is either designated as an input to the circuit or connects to exactly one output terminal of the element 
	- There are no cyclical paths in the circuit

## Boolean Equations 
- Boolean equations show us how the output of a circuit is related to its inputs. 
- A truth table takes $2^N$ rows where N is the number of inputs. 
- Minterms are where we have a logical '1' as the output.
- We can use minterms to express the output as a sum of products 
	- K-Maps
- Maxterms are where we have a logical '0' as the output 
- We can use max terms to express the output as a product of sums

![[Pasted image 20260529202846.png]]
![[Pasted image 20260529202921.png]]
![[Pasted image 20260529202933.png]]

### Multilevel Combinational Logic 
- Logic in sum-of-products form is called two-level logic because it consists of literals connected to a level of AND gates connected to a level of OR gates

### X and Z 
- X represents an unknown or illegal value. 
	- Driven to 0 and 1 at the same time. 
	- Contention
	- Circuit will get hot and damaged. 
- Z Represents a floating value that is neither high nor low. 
	- High Impedance or High Z or Floating. 
	- Common way to produce floating node is to not connect voltage to circuit input, or assume unconnected is same as 0. 

## K-Maps
- Graphical method for simplifying boolean equations 
- Work well for problems with up to four variables 
- Process: 
	- Use fewest circles necessary to cover all the 1s 
	- All the squares in each circle must contain a 1 
	- Each circle must span a black that is a power of 2 (1,2,4,8...)
	- Each circle should be large as possible 
	- Circles can wrap around the edges 
	- A 1 in a K-map can be used multiple times. 
	- your solution is what does not change for each circle 
	- ![[Pasted image 20260530141349.png]]
	- A is 1 and C is 0 $\implies$ $A \bar{C}$

## Combinational Building Blocks 
- Combinational logic is often grouped into larger building blocks to build more complex systems 

### Multiplexer 
- Most commonly used device. 
- MUX 
- Selects a signal

### Decoder 
- Like the opposite of a multiplexer. 
	- Takes N inputs and can output $2^N$ different outputs. 
	- Outputs are one hot because exactly one can be high at a given time 

## Timing 
- There is a delay between an input change and the subsequent output change. 
- Rising Edge is low -> high 
- Falling edge is high -> low 
![[Pasted image 20260531121122.png]]
### Propagation and Contamination Delay 
- Combinational logic is characterized by its propagation delay and contamination delay. 
- The propagation delay is the max time from input change until the output or outputs reach final delay 
- Contamination delay is the min time from when input changes until any output changes in value. 
- These delays are primarily caused by capacitance in the circuit. 
- we focus usually on the critical path, which is the longest and therefore the slowest path. 

### Glitches 
- Single input transition causes multiple output transitions 
- These are called glitches or hazards. 
- 