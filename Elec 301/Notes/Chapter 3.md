## Thevenin, Norton, and Miller's Theorems

### Thevenin's Theorem:
- We can treat a part of a network as just an equivalent voltage source $V_{t}$ and an equivalent impedance $Z_{t}$
- $V_{t}$ , Thevenin equivalent voltage source
- $Z_{t}$ Thevenin equivalent impedance. 
	- To determine the values, we can measure the Thevenin voltage across the two terminals that are desired. This is $V_{t}$
	- To find Thevenin impedance, we short circuit all independent voltage sources, open circuit all independent current sources, and set all initial conditions to zero. Ensure there is no coupling between the two terminals, and then calculate the equivalent Thevenin impedance. 
- Example: 
- ![[Screenshot 2025-12-04 181458.png]]
- To calculate the Thevenin equivalent voltage, we disconnect A from B. 
- ![[Pasted image 20251204181602.png]]
- To obtain the Thevenin equivalent impedance, $Z_{t}$, we conenct a test voltage source, $V_{test}$, to A and measure the current into A, $I_{test}$ as shown.
![[Pasted image 20251204181714.png]]
Simplification: 1A 2A method. 

Since we are assuming a linear circuit, we can describe the voltage across the two terminals as $V= V_{th} + I\cdot Z_{th}$
Then we can replace A with the Thevenin equivalent. 
![[Pasted image 20251204181859.png]]


### Norton's Theorem

A portion of a network can be represented as an equivalent current source $I_{n}$ and an equivalent admittance $Y_{n}$

$I_{n}$ and $Y_{n}$ are the Norton equivalent current source and admittance respectively.
- We can obtain them by opening the circuit at the terminals across which they are desired. 
- Measure the current by shorting the two terminals. (The wire that shorts is the relevant current)
- Short circuit all independent voltage sources, open-circuit all independent current sources. Set initial conditions to zero, check that there is no coupling, then calculate $Y_{n}$ (magnetic coupling).

Example:
1. First we disconnect A from B, then short the two terminals. 
![[Pasted image 20251204182335.png]]
![[Pasted image 20251204182357.png]]
