## Amplification Principle 
- Separate input and output ports 
- Energy is transferred from DC (battery) to signal through inherent nonlinearity of the device. 


## Linear Circuits 
- Simplifies circuit analysis if circuit is linear 
- LITI
	- Linear implies we can apply superposition and homogeneity. 
	- Time-invariant states that the structure of the system does not change in time. 
	- LTI allows the design and analysis in either time or frequency domain  
- Good feedback allows for linearization of a system. 

## Time-Invariant Systems 
- Time invariant tells us that a time shift in the input signal produces an identical time shift in the output. 
- Heuristics: the structure of the system remains the same, regardless of when input is applied. 
- Relative difference of time between input and output. 

## Causality 
- System where output depends only on input at present or past. Not future. 

## Linearity 
- A linear system means its action can be described by a linear operator H 
- Superposition and homogeneity properties. 
- we can use superposition as a tool to solve seemingly complex circuits. 

## Circuit Transformations 
- Recall Thevenin/ Norton equivalent circuit theorems for linear uniports. 
- Application: graphical reduction of branches. 

## Reduction Techniques 
- Superposition Theorem: current through or voltage across any element of a network is the algebraic sum of currents and voltages independently produced by each. 
- Voltage sources become short circuit
- Current sources become open circuit. 

## Thevenin Theorem: 
- Any two terminal linear network can be replaced by equivalent circuit consisting solely of voltage source and series impedance. 

### Thevenin Impedance 
- Set all sources to 0. 
- Determine the equivalent impedance as seen by the ports. 


### Thevenin Voltage 
- Keep all sources as they originally were. 
- Assume time dependent components are in their initial un-interacted with state. 
- Find voltage between the two ports. 

#### External View 
- Any portion of a linear circuit w.r.t. a port can be 