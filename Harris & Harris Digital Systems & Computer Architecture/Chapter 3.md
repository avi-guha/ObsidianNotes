# Sequential Logic Design 
- Sequential logic depends on both current and prior input values 
- output of sequential logic depends on both current and prior input values 
- Sequential logic has memory 
- The state of digital sequential logic is a set of bits called state variables. 
	- Contains info about the past to explain future behaviour of the circuit 

## Latches and Flip Flops
- Fundamental building block of memory is a bistable elements 
	- element with two stable states 
- ex. inverters that are cross-coupled 
- ![[Pasted image 20260531150620.png]]
-  **Analysis:**
	- Case 1: 
		- Q is 0 
			- Means that $\bar{Q}$ is 1
				- Then Q is 0 
		- means that it is stable 
	- Case 2: 
		- Q is 1
			- Means that $\bar{Q}$ is 0 
				- Then Q is 1 
		- Means that it is stabled 
	- Because there are two stable states, it means that it is bistable 
	- A third state where both are between 0 and 1 exists in which it is called metastable 
- An element with N stable states conveys $\log_{2}N$ bits of information 
	- bistable element stores one bit.
- When power is first applied to sequential circuit, the initial state is unknown and usually unpredictable 
	- Differs each time the circuit is turned on. 

### SR Latch
- An SR latch is a 1-bit memory element controlled by Set (S) and Reset (R).
- It stores its previous value when both inputs are 0.
- The input combination $S = 1$ and $R = 1$ is invalid because it tries to set and reset the latch at the same time.

| S | R | $Q_{next}$ | Operation |
|---|---|------------|-----------|
| 0 | 0 | Q          | Hold      |
| 0 | 1 | 0          | Reset     |
| 1 | 0 | 1          | Set       |
| 1 | 1 | X          | Invalid   |

### D Latch
- A D latch is a 1-bit memory element with one data input, D, and an enable or clock input.
- When the enable is 1, the latch is transparent, so Q follows D.
- When the enable is 0, the latch stores its previous value.
- avoids the issue of S and R being high at the same time. 
- Uses a Data input D and a clock input (EN)

| EN | D | $Q_{next}$ | Operation |
|----|---|------------|-----------|
| 0  | X | Q          | Hold      |
| 1  | 0 | 0          | Load 0    |
| 1  | 1 | 1          | Load 1    |

### D Flip Flop
- A D flip flop is an edge-triggered 1-bit memory element.
- On the active clock edge, usually the rising edge, it copies D to Q.
- Between active clock edges, it holds its stored value.

| Clock          | D   | $Q_{next}$ | Operation |
| -------------- | --- | ---------- | --------- |
| No active edge | X   | Q          | Hold      |
| Rising edge    | 0   | 0          | Load 0    |
| Rising edge    | 1   | 1          | Load 1    |

# D Latch vs. D Flip-Flop

> [!summary] The Core Difference
> The fundamental difference comes down to **when** they sample input data. A **D Latch** is level-sensitive (continuous), while a **D Flip-Flop** is edge-triggered (instantaneous).

## Comparison Breakdown

| Feature | D Latch | D Flip-Flop |
| :--- | :--- | :--- |
| **Trigger Mechanism** | **Level-sensitive:** Controlled by an Enable (EN) signal. | **Edge-triggered:** Controlled by a Clock (CLK) signal. |
| **Active Behavior** | **Transparent:** As long as Enable is HIGH (1), the output (Q) continuously follows the input (D). | **Momentary:** The output (Q) only captures the input (D) at the exact instant the Clock transitions (e.g., rising edge). |
| **Inactive Behavior** | **Opaque:** When Enable is LOW (0), it locks and holds the last state. | **Locked:** For the entire duration of the clock cycle, it holds its state, ignoring any input changes. |
| **Analogy** | A door held open. People can walk in or out until the door shuts. | A camera shutter. It captures a quick snapshot at a precise millisecond. |
| **Primary Use Case** | Clock gating, asynchronous design, or simple memory elements. | Synchronous logic systems, hardware registers, and processors. |

## Architectural Note
A standard D Flip-Flop is actually built using two D Latches connected in series (a "Master-Slave" configuration) with an inverted clock between them. This isolates the input from the output, ensuring data only steps forward exactly one stage per clock edge, preventing race conditions in complex circuits.

### Register
- A register is a group of flip flops that store multiple bits at the same time.
- All bits usually share the same clock, so the whole binary value updates together on the active clock edge.
- can represent number up to $2^{n-1} +1$

| Clock | D[n:0] | $Q_{next}$[n:0] | Operation |
|-------|--------|-----------------|-----------|
| No active edge | X      | Q[n:0] | Hold |
| Rising edge    | D[n:0] | D[n:0] | Load |

### Enabled Flip Flop
- An enabled flip flop is a D flip flop with an enable input.
- It only loads D on the active clock edge when EN is 1.
- If EN is 0, it holds its previous value even when a clock edge occurs.
- basically a gated clock

| Clock | EN | D | $Q_{next}$ | Operation |
|-------|----|---|------------|-----------|
| No active edge | X | X | Q | Hold |
| Rising edge    | 0 | X | Q | Hold |
| Rising edge    | 1 | 0 | 0 | Load 0 |
| Rising edge    | 1 | 1 | 1 | Load 1 |

### Resettable Flip Flop
- A resettable flip flop is a D flip flop with a reset input that forces Q to 0.
- Reset is used to put sequential logic into a known starting state.
- If reset is asynchronous, it affects Q immediately; if reset is synchronous, it only affects Q on the active clock edge.

| Reset | Clock | D | $Q_{next}$ | Operation |
|-------|-------|---|------------|-----------|
| 1 | X | X | 0 | Reset |
| 0 | No active edge | X | Q | Hold |
| 0 | Rising edge    | 0 | 0 | Load 0 |
| 0 | Rising edge    | 1 | 1 | Load 1 |

### Transistor-Level Latch and Flip Flop Designs 
- Flip flops require a large number of transistors when built from logic gates. 
- A compact D latch can be constructed from a single transmission gate 
	- Suffers limitations 
		- Floating output node: when latch is opaque, Q is not held at value by any gates so it is a floating or dynamic node 
		- No buffers: lack of buffers causes malfunctions on commercial chips. 


## Synchronous Logic Design 
- In general, sequential circuits include all circuits that are not combinational 

### Astable Circuits 
- A circuit that is inconsistent with itself is said to be astable. 

### Synchronous Sequential Circuits 
- Cyclic paths have outputs that are fed directly back to inputs. 
- Sequential circuits with cyclical paths can have undesirable races or unstable behaviour. 
- To avoid problems, designers insert registers somewhere in the path. 
	- Transforms the circuit into a collection of combinational logic and registers. 
- Registers contain the state of the system, so the state is synchronized to the clock. 
- A sequential circuit has finite set of discrete states 
- A synchronous sequential circuit has a clock input, whose rising edges indicate a sequence of times at which state transitions occur. 
- Often use the terms current state and next state to distinguish the state of the system at the present from the state to which it will enter on the next clock stage.
- The rules of synchronous sequential circuit composition: 
	- Every circuit element is either a register or a combinational circuit 
	- At least one circuit element is a register 
	- All registers receive the same clock signal 
	- Every cyclic path has at least one register. 

### Synchronous and Asynchronous Circuits 
- Asynchronous design in theory is more general than synchronous design, because the timing of the system is not limited by clocked registers. 
- Virtually all digital systems are essentially synchronous. 
- Asynchronous circuits are occasionally necessary when communicating between systems with different clocks or when communicating systems with different clocks or when receiving inputs at arbitrary times. 

## Finite State Machines
- synchronous sequential circuits can be drawn as finite state machines 
- They have k registers and have a finite number of outputs $2^k$
- FSM has M inputs, N outputs, and k bits of states. 
- Receives a clock and optionally a reset. 
- FSM contains two blocks of combinational logic, next state logic and output logic, and a register that stores the state 
- On each clock, the FSM advances to the next state, which is computed based on current state and inputs. 
- There are two general classes of FSM 
	- Moore machines - depend only on current state 
	- Mealy machines, depend on current state and current inputs 

### Example: 
- Controller for traffic light at an intersection 
- state transition diagram: 
	- ![[Pasted image 20260601133030.png]]
		- circles represent states 
		- arcs represent transitions between states 
		- transitions take place on the rising edge of the clock 
		- we can write a state transition diagram as a table and give it binary encodings. 
	- To build outputs we must assign the states and outputs to binary codings
	- ![[Pasted image 20260602002610.png]]
To translate this high-level state machine behavior into digital logic hardware, you need to replace the abstract state names with their binary values to derive the boolean equations for the next-state logic. 

We can simplify state tables with karnaugh maps: 
![[Pasted image 20260602211820.png]]



Here is the step-by-step process for deriving the equations for this specific system.

### Step 1: Create an Encoded Truth Table

First, we define our variables. Let the current state bits be $S_1$ and $S_0$, and the next state bits be $S_1'$ and $S_0'$. The inputs remain $T_A$ and $T_B$.

By substituting the state names in Table 3.1 with the binary encodings from Table 3.2, we get a combined truth table. An "X" means "Don't Care," indicating that the specific input does not affect the outcome for that row.

| Current State ($S_1, S_0$) | Inputs ($T_A, T_B$) | Next State ($S_1', S_0'$) |
| :------------------------- | :------------------ | :------------------------ |
| 0 0 (S0)                   | 0 X                 | 0 1 (S1)                  |
| 0 0 (S0)                   | 1 X                 | 0 0 (S0)                  |
| 0 1 (S1)                   | X X                 | 1 0 (S2)                  |
| 1 0 (S2)                   | X 0                 | 1 1 (S3)                  |
| 1 0 (S2)                   | X 1                 | 1 0 (S2)                  |
| 1 1 (S3)                   | X X                 | 0 0 (S0)                  |

---

### Step 2: Derive the Equation for the Most Significant Bit ($S_1'$)

To find the boolean equation for $S_1'$, we only look at the rows where $S_1'$ is **1** in the Next State column. 

1.  **Row 3:** $S_1'$ is 1 when the current state is 01 ($S_1=0, S_0=1$). Because the inputs are "Don't Cares", this happens regardless of $T_A$ or $T_B$. This gives us the term: $\overline{S_1} S_0$
2.  **Rows 4 & 5:** $S_1'$ is 1 when the current state is 10 ($S_1=1, S_0=0$). Notice that it is 1 when $T_B=0$ *and* when $T_B=1$. Because it is 1 in both cases, $T_B$ drops out of the equation. This gives us the term: $S_1 \overline{S_0}$

Combining these terms yields the sum-of-products equation:

$$S_1' = \overline{S_1} S_0 + S_1 \overline{S_0}$$

This specific configuration is the exact definition of an Exclusive-OR (XOR) gate, meaning the equation can be simplified to:

$$S_1' = S_1 \oplus S_0$$

---

### Step 3: Derive the Equation for the Least Significant Bit ($S_0'$)

Next, we repeat the process for $S_0'$, looking only at the rows where $S_0'$ is **1**.

1.  **Row 1:** $S_0'$ is 1 when the current state is 00 ($S_1=0, S_0=0$) **AND** the input $T_A$ is 0. This gives us the term: $\overline{S_1} \overline{S_0} \overline{T_A}$
2.  **Row 4:** $S_0'$ is 1 when the current state is 10 ($S_1=1, S_0=0$) **AND** the input $T_B$ is 0. This gives us the term: $S_1 \overline{S_0} \overline{T_B}$

Combining these terms gives the equation for $S_0'$:

$$S_0' = \overline{S_1} \overline{S_0} \overline{T_A} + S_1 \overline{S_0} \overline{T_B}$$

If you were building this with discrete logic gates, you could optionally factor out $\overline{S_0}$ to save a logic gate, yielding: $S_0' = \overline{S_0} (\overline{S_1} \overline{T_A} + S_1 \overline{T_B})$.


### State Encodings 
- Before, state and output encodings are selected arbitrarily 
	- Different choice results in different circuit 
	- We should worry about what the best way to determine encodings is 
- There is no simple way to determine good state encodings 
	- usually cad tools are better at this
- With binary encoding, each state is represented by a binary number so with K binarynumbers we can represent $\log_{2}K$ 
- In one-hot encoding, a separate bit of state is used for each state 
	- This is called one hot because only one bit or state is active at a time. 
- The best encoding depeneds on the FSM 

### Moore and Mealy Machines 
- ![[Pasted image 20260602212333.png]]
- in the mealy machine the / refers to the input/output 
	- first number is input that triggers the change 
	- second number is the output the machine produces while making the transition 

### Factoring State Machines 
- Designing complex FSM is easier if we can break it into smaller 

### FSM Reviews 
- Process: 
	- Identify inputs and outputs 
	- Sketch state transition diagram 
- For Moore machine 
	- State transition table 
	- write an output table 
- For Mealy machine 
	- Write a combined state transition and output table 
- Select state encodings - selection affects hw design 
- Write boolean equations for next state and output logic 
- Sketch schematics 

### Timing of Sequential Logic 
- We can recall a flip-flop copies the input D to the output Q on the rising edge of the clock 
	- This is called sampling D on clock edge 
	- if D is stable at either 0 or 1 when clock rises this is defined behaviour. 
		- What happens if D is also changing

- We can write A[n] as the value of a signal A at the nth clock cycle 

### Dynamic Discipline 
- When a clock rises, the output may start to change after the clock-to-Q contamination delay, $t_{ccq}$ and must definitely settle to the final value within the clock to q propagation delay $t_{pcq}$. 
- For a circuit to sample input correctly, inputs must have been stabilized in some setup time $t_{setup}$ before the rising edge and must remain stale for at least some hold time after the rising edge of clock 
- The dynamic discipline state that inputs of a sequential circuit must be stable during setup and hold aperture time around a clock edge 

### System Timing 
- The clock period or cycle time is the time between rising edge of a repetitive clock signal. 

#### Setup time constraint 
- $T_{c} \geq t_{pcq} + t_{pd} +t_{ setup}$
- We can rearrange to find the max propagation delay 
- $t_{pd} \leq T_{c} - (t_{pcq} + t_{setup})$
	- Brackets term is the sequencing overhead
	- Equation is called the setup time constriant or max delay constraint 


### Hold time constraint 
- $t_{hold} \leq t_{ccq}$

### Putting it all together 
- sequential circuits have setup and hold time constraints that dictate the max and min delays of the combinational logic between flip flops 

### Clock skew: 
- Sometimes the clock propagation takes a little bit of time in between the registers. 


### Metastability 
- it is not always possible to guarantee input to a sequential circuit is stable during aperature time. 

#### Metastable state 
- when a flip flop samples an input that is changing during its aperture, output may take a voltage between 0 and high 
	- this is called metastable 
	- eventually flip flop resolves to the stable state 

#### Resolution time 
- If a flip flop changes input at a random time duirng the clock cycle, the resolution time, $t_{res}$ required to resolve a stable state is also a random variable 
- $P(t_{res} > t) = \frac{T_{0}}{T_{c}}e^{-t/\tau}$


### Synchronizers 
- Asynchronous inputs to digital systems form the real world are inevitable 
- To guarantee good logic levels, all synchronous inputs should be passed through synchronizers 

### Parallelism 
- The speed of a system is measured in latency and throughput of tokens moving through a system. 
- A token is a group of inputs that are processed to produce a group of outputs. 
- The throughput of a system is the number of token that can be produced per unit time. 

- We can maximize throughput by processing several tokens at the same time 
	- parallelism
		- spatial parallelism 
			- multiple copies of hardware are provided so multiple copies can be done in tandem 
		- temporal parallelism 
			- a task is broken into stages, like an assembly line. Multiple tasks spread across stages.
			- Though each stage does one at a time a different task is done at different stages at different times 
			- This is known as pipelining 
