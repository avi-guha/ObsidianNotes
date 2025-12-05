
## Failure at high and low Applied Stress 
- Failures can occur at applied stress levels above the yield strength (plastic deformation, eventual fracture).
- Failure can also occur at applied stresses well BELOW the yield strength (creep, fatigue, or fracture accelerated by cracks and flaws)
- Understanding how failure can occur at relatively applied stresses is critical to determining safe component design. 
- We examine 4 Areas of failure
	- Fracture (catastrophic breaking under constant load due to an initial crack)
	- Fatigue (a rack grows over repeated oscillating cycles of loading)
	- Creep ( a constant load at elevated temperatures can continue to deform)
	- Environmental degradation / corrosion    
- Aside 
	- An applied tensile stress is amplified at the tip of a small incision or notch 

Visual Characteristics $\textemdash$ Ductile vs. Brittle Fracture 

![[Pasted image 20251006122615.png]]
- Ductile fractures are stable fractures $\textendash$ tend to resist rupture under larger stresses by hardening and deforming 
- Brittle fractures are unstable fractures $\textendash$ tend to proceed catastrophically under larger stresses.

- Ductile Fracture $\textemdash$ "Cup and Cone" Fracture.
![[Pasted image 20251006122900.png]]

Question: ![[Pasted image 20251006122936.png]]
Answer: 
- Necking begins
- Cavities enlarge due to plastic deformation
- Cavities Coalesce to form a crack
- Crack propagates toward the surface along a shear plane
- When applied stress can no longer be sustained, final fracture occurs.
C. 

## Ductile Fracture
- Scanning electron microscope fractography (images of fracture) show an irregular, fibrous appearance due to plastic deformation. 
- ![[Pasted image 20251006123238.png]]

## Brittle Fracture 
- Brittle fracture is usually flat and perpendicular to the stress axis. The fracture surface is shiny, with a grainy appearance. 
- Fracture lines within the grains ( along crystal planes) or along grain boundaries, depending on the material and processing (heat treatments, chemical attack on boundaries, etc. )
- ![[Pasted image 20251006123641.png]]
- ![[Pasted image 20251006123659.png]]

## Summary of Fracture Characteristics 

|  Ductile $\textemdash$ High Toughness                                | Brittle $\textemdash$ Low Toughness                                                                                                           |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Extensive Plastic Deformation                                        | Minimal Plastic Deformation                                                                                                                   |
| The fracture surface appears grey and fibrous and dull               | Fracture structure is crystalline (shiny, due to intra-granular or trans-granular fracture)                                                   |
| "Dimpled" or cup and cones morphology on fracture surface            | Flat Surfaces                                                                                                                                 |
| Evidence of shear $\textendash$ failure along paths of maximum shear | Evidence of Cleavage of Grains<br>- Chevron patterns with fracture surface perpendicular to load<br>- Fan-shaped pattern on fracture surface. |
$\textbf{Ductile Fracture is Generally Tougher }$, it generally absorbs more energy before failure than brittle fracture ( plastic deformation requires energy input)

## Ductile to Brittle Transition Temperature  (DBT)
- The drastic reduction in ductility of a material within a temperature range. 
- Ductile to brittle transition is observed in: 
	- BCC steels (most widely used)
	- Some HCP and BCC metals
	- Many Ceramics
	- Many polymers 
- FCC metals (Al, Cu, Ni, Stainless steels) do not exhibit DBT.

- Ductility (% elongation) is used as an indicator of whether a sample is ductile or brittle 
- More ductile as temperature increases
- There is a sharp transition in s small transition point 
	- Called the DBTT 
![[Pasted image 20251006125134.png]]
- If we redo tests at a higher strain rate of 1 $s^{-1}$ 
- ![[Pasted image 20251006125210.png]]
- #### Take aways 
	- Brittle behavior starts at a higher temp as the strain rate increases 
	- Therefore, higher strain rates are qualitatively similar to running experiments at a lower temperature 
	- The highest order of $10^{6}s^{-1}$ ie. shock or impact loading $\textemdash$ is ultimately limited by how fast stress waves can travel (speed of sound in the material).

## <u>Impact Testing</u> of DBT
- We want a high strain rate test in order to know how a material might behave under the worst type of service conditions $\rightarrow$ impact loading
- Therefore we use an impact test. Several types exist, we focus on the Charpy impact test
- In many ways, this is the worst case for service condition: notched sample, under impact, low temperature. 
- Test measures: "Charpy V-Notch Toughness" 
- CVN allows for comparative testing of materials, but does not quantitatively measure the toughness of the material 

### Introduction to the Charpy Impact test
- Tensile tests are difficult to perform at high strain rates, and therefore can be poorly representative of fracture behavior
- Impact tests are conducted under severe service conditions: 
	- High strain rate
	- Low temperature
	- Triaxial stress state (from notch of specimen)
	- Charpy and Izod impact tests are commonly used 
	- Charpy uses a standard specimen size ( 10 x 10 x 55) \[mm] with a 2mm notch. 
	- ![[Pasted image 20251006131010.png]]
	- ![[Pasted image 20251006131026.png]]
	- ![[Pasted image 20251006131040.png]]
	- ### Questions: 
	- ![[Pasted image 20251006131105.png]]
		- A, C, D Correct 
	- ![[Pasted image 20251006131340.png]]
	- $E_{absorbed} = 10 kg * 10 \frac{m}{s^{2}} * (1.5 - 0.9) m = 60J$ 
	- Assumptions: 
		- Losses to air drag, pivot friction vibration and sound are negligible
		- The hammer's center of mass is at h1, and h3.
		- g = 10 $m/s^2$
		- All energy difference is absorbed by specimen (plastic deformation and fracture)

## Fracture Surfaces: A diagnsis
- Fracture surface appearance:
	- Brittle fracture is shiny (cleavage fracture)
	- Ductile region is dull and fibrous (shear fracture)
- In addition to CVN energy absorption, one can characterize failure to %(shear fracture) or %(ductile fracture region)
- ![[Pasted image 20251006131654.png]]
- A36 steel
![[Pasted image 20251006131713.png]]
![[Pasted image 20251006131727.png]]
- #### notes
	- Yield stress increases
	- DBTT at higher temperature 
	- Shelf energy decreases (fracture energy above DBTT)

## Specification of Transition Temperature and Design 
- CVN number cannot be used to calculate a 'safe stress' in design calculations
- Useful for specification of minimum CVN energy, quality control and comparison of similar materials 

## Many FCC Metals are always ductile and do not exhibit DBT
- FCC metals are low-strength but have high impact energy
- High-strength materials are brittle, and with low impact energies 
- Ductile failure is a result of dislocation movement, brittle fracture occurs as a result of crack propagation 
- Whether ductile or brittle failure occurs depends upon which mode of failure is associated with a lower energy 
- For most BCC materials, energy required for dislocation movement is highly temperature-dependent, whereas that for FCC materials is not temperature dependent 
- FCC materials do not demonstrate DBT 
- ![[Pasted image 20251006132128.png]]\