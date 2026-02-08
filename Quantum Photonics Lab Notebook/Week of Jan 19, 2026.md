
# Monday: RF Electronics

### Design Layout Simulation of Inset Fed Microstrip Patch Antenna

#### Concepts
- **Inset Fed Patch Antenna:** A microstrip antenna where the feed line is 'inset' into the patch to match input impedance (~50 $\Omega$) without needing external matching network. 
- **Software:** Video uses ADS for layout and specific FEM (Finite Element Method) for 3D electromagnetic simulation. I will likely be using cadence allegro. 
- **Fringing Field Effect:** Theoretical formulas often assume 'perfect' walls, but in reality fringe effects at edges make antennas electrically larger than physical size. This results in a resonant frequency lower than calculated frequency. 
- 
#### Monday Aside: Signals from SNSPD:
- Question: How does polarization impact the output signal from an SNSPD


### Tuesday: Meeting with Jeff and Adan
- True Cryogenic Amplifier: Designed and tested for low temperature. 
- Want to use a surge protecting diode? 
	- Bias T on PCB 
	- Source outside cryostat
	- Amplifier @ 4K 
	- potentially @ 1K (device itself?)
		- measure short nanowires 
	- Bias T, Low noise voltage source
		- inside cryo minimizes jitter. 
		- powerful to put things on PCB, faster to change? 
		- Why?
			- External inductor high resistivity, not superconducting. 
			- series resistor, can reduce recovery time? 
			- Latching - never returns to superconducting 
				- Current diverging from S-N to shunt, if inductance not high enough in wire results in this. 
				- impedance matching for minimal reflections. 
				- shorter wire, lower inductance but latches more.
				- longer wire, higher inductance but latches less. 
			- More generic PCB is better. Specialize later. 
			- considering superconducting inductor. 
			- Solder for PCB at cryo? 
				- Maybe lead will work maybe it won't
- 

### Project 1: starting
- PPMS Probe 
- Length and Diameter are constraints 
- Copper likely, not too thick but thick enough to stop photons from absorbing and producing radiation inside 
- Goal: Draft produced by Jan 28, 2026

### Thursday: Prove Design Notes 
- Reduces thickness of center material from 0.55 cm to 0.35cm. 
	- Allowed for 4 SMP connectors and easier bolt screw in
- Designing copper faraday cage 
	- settled on 1mm thickness (do I have 2 mm of leeway to increase the bottom and upper plate? probably)

## Friday: Cross checking with Adan + QKD

