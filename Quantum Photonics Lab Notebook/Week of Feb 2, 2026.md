## New PPMS Probe Design
- working on new PPMS probe design - improving for stability and accounting for thermal expansion etc. 

### Thermal Expansion Coefficients 
- Aluminum: 
	- $\alpha = 23 \cdot 10^{-6} \; \celsius ^{-1}$
- Copper: 
	- $\alpha = 18 \cdot 10^{-6} \; \celsius^{-1}$

### Relevant Shrinking Calculations: 


## Redesigning PCB for Application to center the SMP Connectors 

Allocate 14mm x 28 mm for chip.

![[Pasted image 20260205222804.png]]

Footprints: 
Mounting Holes: MountingHole:MountingHole_2.2mm_M2
Connectors: SMP_SMT_molex:MOLEX_85305-0232
- key dimensions of new center-SMP PCB: 
	  - 11.3 mm width 
	  - 50.2mm length
	  - max height 5.73mm in centerline

## Meeting 
- Might / might not work 
- Finish off the probe ASAP 
- dummy test 
- maybe make the pcb thing in the free time. 
- IV curve - sweeping voltage. 
	- 10mV sweeps.
	- 0->1 , 0- -1, -1 -> 0
- SNSPD performance 
	- 10 mV sweeps 