
# Monday 
- Touring around
- Description of tasks
- Overview of the cryogenic, and SNSDP, task of developing the PCB to take signal from the SNSPD through the SMP given. 
- Finished majority of workplace safety training.
- Goal for Tuesday: finish workplace safety and start research on RF design. 

# Tuesday: 
- Finished LASER module. 
- Looked over the PCB file from *Edward Leung*, familiar with some design choices:
	- Via fencing (faraday cage for the signal)
	- Symmetry, keeps phase balance in RF design
	- Some design choices seem sub-optimal for high frequency design?
		- Sharp bends
			- Can cause signal reflection due to a drop in trace width (higher impedance)
		- Thermal Relief for connections on pads 
			- Parasitic inductance, might be better to use a solid copper pour, though this is more difficult for manufacturing. 
		- Large signal pads $\rightarrow$ high parasitic capacitance 
			- Could use a ground cutout there. 
		- Not sure if there is an intended termination impedance (the trace width and gap changes a bit which can make this inconsistent)
- Looking into RF Design for PCBs, notes below. I will be taking notes on this Youtube playlist for the next 2-3 days: https://www.youtube.com/playlist?list=PL9OnCetH8TYpFk-gC0SRB9izEIblJmjBm
- Also Looking into SNSPD to make more sense of the project, the type of signals I will be dealing with etc. 

# Tuesday - Thursday
## Notes on SNSPD Detector
- Design around 10 GHz
	- Most likely 2-3 GHz 
	- Majority of signal will lie in the 100-400 MHZ
- SNSPD should look like gaussian decay in Fourier domain 
- Can assume the tail is the around 3GHz  (cutoff frequency)
- SNSPD LTSpice: https://github.com/qnngroup/snspd-spice
- Bias junction
	- Not just the bias T
		- RF line vs DC
	- This limits the current we can send through. 
- Why SNSPD? 
	- Jitter is 3ps 
	- Latency has GHz Potential. 
	- Compare to TES (Transition-Edge Sensor)
		- Jitter is much higher (ns)
		- Much more precise than the SNSPD 
	- Very high speed too (maximal count rate of 2GHz)
- Cooper Pairs $(e \times e) = \left\{  \left( +\frac{1}{2}, +\frac{1}{2} \right), \left( +\frac{1}{2}, -\frac{1}{2} \right),  \left( -\frac{1}{2}, -\frac{1}{2} \right)   \right\}$ Gives us spin = -1,0,1 bosons. 
	- When a photon absorbs into the superconducting nanowire, the cooper pairs break and critical current falls below the bias current. 
	- This results in a region of the nanowire becoming non-superconducting.
	- Finally, the presence of a non-superconducting region results in a voltage pulse of 1ns. 
- Question: what role does Johnson-Nyquist noise play in this (when we absorb a photon surely we have some agitation of charge carrier)?
	- How can we minimize this? (colder, feedback mechanism, etc.  high-pass/low-pass filter at the rate of the noise with $\omega_{l_{3dB}} = JN_{noise}$
	- Why High Pass / Low pass
		- I do not know what J-N noise will look like, so I need to see what the ideal operating region will be. 
- Disadvantage: lack of intrinsic energy resolution. This means that we cannot precisely measure the energy arising from fluctuations in the initial photon.
	- Is there any way that if I measure both of the resulting pulses I could take the difference to determine where the noise is coming from? 
	- Measure the positive and negative ? 
		- If noise is constant (then there is no difference that results from this)
		- Basically need to characterize the noise 
- If we can get complete darkness, the number of dark counts would be 0. This is probably the point of the faraday cage? 
- We have high max counting rate, so data transmission will be high. 
- Dead time: time between the photon being absorbed, resistivity period, and the wire cooling down again.
- Detection side: 
	- 

### From Gemini:
1. Superconducting state, wire is far below superconducting temperature, and biased with constant electrical current, below its critical current. (zero resistance)
	1. critical current is the max current density it can carry before losing zero resistance state
	2. If we exceed critical current, we lose the superconductive state 
		1. Concepts: 
			- Cooper pairs 
				- move without resistance (if current increases, these move fast enough to break apart)
			- Magnetic field generation: 
				- Magnetic field due to a large enough current could destroy superconductivity 
			- Vortex Pinning? 
				- Not sure if relevant 
				- Magnetic flux lines can penetrate, when Lorentz force exceeds pinning of flux vortices in place. they move, causing resistance 
			- Critical current density 
				- current per unit area, depends on factors such as T, and B and material properties
2. Photon Absorption. When a single photon hits the nanowire, it deposits energy, breaking apart 'cooper pairs'
	1. Photon energy must have enough energy to break apart the cooper pairs 
	2. $E_{pair} = 2 \Delta$
		1. $\Delta$ is the superconducting energy range (~2.3 meV for NbTiN)
		2. for microwave region of interest 
			1. The photons frequency is quite high (THz)
3. Hotspot Topology Formation 
	1. Energy creates a tiny local region of non-superconducting resistive material called a hotspot
	2. The propagation of cooper pairs breaking increases the size of this hotspot
4. Current Redirection: since the hotspot has resistance in the range of $100k \Omega$, we use a shunt to redirect the current and also allow the region to cool down (cooper pairs  reform?)
5. Voltage pulse: creates a measurable voltage pulse in the range of 3GHz (assumed)
	1. Rising edge $\rightarrow$ Current Redirection 
		- Photon creates hotspot, resistance jumps 
		- Shunting: readout circuit has relatively lower resistance, so the bias current is shunted from nanowire to the readout electronics 
		- When we measure $V_{out}$ it is a small signal, low voltage (need cryogenic amplifiers?)
	2. Falling Edge: Kinetic Inductance 
		- Gaussian decay  
		- This determines the dead time of the SNSPD
		- Decay follows curve with the following time constant $\tau_{fall} = \frac{L_{k}}{R_{L}}$
		- From Mateo: 
			- Longer wire, better detection efficiency, slower cooldown 
			- Shorter wire, worse detection, faster cooldown 
			- Limits our count rate.
	3. Thermal reset 
		- The wire must reset to superconductive state, while there is no current flowing through it, the cold substrate pulls the heat away. 
		- Once temperature falls below critical temp, the resistance vanishes, and kinetic inductance pull current back from readout circuit into wire. 
6. Recovery: hotspot cools down and material regains superconductivity, then the detector is ready for the next photon again. 
		

## RF Design (Microwave Engineering): 


# Wednesday Aside Notes 
- Design Specifications added: 
	- Solder must be able to withstands thermal changes 
	- Reflow for placement of SMP connectors 
	- Faraday cage surrounding will block out other forms of noise in the cryo
		- Is vibrational noise an issue (how can we address)
		- Light noise, electronic noise etc. 


# Thursday Aside 
- Kinetic Inductance 
	- Concept: Momentum as Inductance 
		- In superconductors, since there is zero resistance, the cooper pairs (charge carriers) can be accelerated to high speeds. 
			- Since cooper pairs are pairs of electrons, which have mass they have kinetic energy. 
		- To change the current, you must change the velocity of these masses. 
			- The inertia of these charge carriers opposes changes in current, similar to an inductor. 
		- Mathematically, this can be modeled with the following equation: 
		- $$\frac{mv^{2}}{2}= \frac{L_{k}I^2}{2}$$
	- Why is it an issue in SNSPDs:
		- In normal copper wires, we still have kinetic inductance, but is is billions of times smaller than magnetic inductance so it is ignored. 
		- Causes: 
			- Low carrier density 
				- NbTiN has less charge carriers than copper. To get the same current, the few carriers must move faster which massively increases $T$
			- Nanoscale dimensions 
				- Since the wire is very thin, it forces the current into a tiny cross section, which increases velocity and resulting $L_{k}$
	- Implications for the PCB: 
		- Slow reset (gaussian decay) is determined by kinetic inductance. 
		- Impedance matching the 100-1000nH wire rather than a simple shunt resistance.
	
