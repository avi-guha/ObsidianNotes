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
- Any portion of a linear circuit w.r.t. a port can be reduced to an equivalent Thevenin voltage source or equivalent impedance $Z_{t}$

#### 1A 2A Method.
- Sent a 1A current source through the circuit, and examine the resulting voltage across the diport.
$$V = V_{Th} + IR_{Th}$$


## Norton's theorem
- Any 2-terminal network can be replaced by an equivalent circuit consisting of a current source and a parallel impedance.

### Norton Impedance 
- Set all sources to zero, and find the resulting impedance between the two ports. 
### Norton Current 
- Calculate the Norton current by returning all sources to original position and finding the short-circuit current between a and b. 


### External View 
- A portion of linear network may be represented at an electrical port by an equivalent circuit source $I_n$and equivalent admittance $Y_{n}$

## Converting between Thevenin and Norton Equivalent Uniports. 
![[Pasted image 20251205070142.png]]


## Millman's theorem 
- Any number of parallel branches can be reduced to one. 
- ![[Pasted image 20251205070255.png]]
$$Y_{eq}(s) = Y_{1} + Y_{2} + Y_{3}$$
- is the same as 
$$\frac{1}{Z_{eq}(s)}= \frac{1}{Z_{1}} + \frac{1}{Z_{2}} + \frac{1}{Z_{3}}$$
$$E_{eq}(s) = \frac{Y_{1}E_{1} + Y_{2}E_{2} + Y_{3}E_{3}}{Y_{1} + Y_{2} + Y_{3}}$$

### Millman's theorem in Series 
![[Pasted image 20251205070606.png]]
$$Z_{eq}(s) = Z_{1} + Z_{2} + Z_{3}$$
$$I_{eq}(s) = \frac{Z_{1}I_{1}+Z_{2}I_{2}+Z_{3}I_{3}}{Z_{1}+Z_{2}+Z_{3}}$$

## Superposition with linearly dependent sources 
- Identify sources, dependent vs independent.
- Activate one independent source, do not disable the dependent sources. 
- Repeat for all independent. 

## Miller's Theorem 
- Helps in analysis by eliminating the feedback. 
- ![[Pasted image 20251205070857.png]]

This works in scenarios where $V_{2} = kV_{1}$ specifically. 

- We can say that $Z_{1} = Z \frac{1}{1-k}$ and $Z_{2} = Z \frac{k}{k-1}$
- ![[Pasted image 20251205071029.png]]