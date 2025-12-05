
# Fatigue 
- Occurs in structures subject to dynamic and varying stress - metals, composites, ceramics, plastics. 
- Failure at stress levels below yield or tensile strength of the material. 
- Fatigue is the largest cause of failure 
- Occurs after a long time of cyclic stress 
- Three stages 
	- Initiation
	- Propagation 
	- Final Fractures 
![[Pasted image 20251103180939.png]]

## Cyclic Loading 
- Cyclic loading is the application of repeated or fluctuating stresses and strains to a component 
- What types of cyclic loading would components in a jet engine faces 
	- Start-up (low temperature to high temperature)
	- Take off, cruise, landing (high load, low load, high load)
	- Rotation - blade flutter/vibration as the stators are passed 

![[Pasted image 20251103181137.png]]

## Diagnosis 
- Beachmarks: found where crack propagation occurred (A); beachmark spacing is indicative of the rate of crack propagation 
- Rapid failure occurred in the rugged section (B)
- Crack initiated at a location of maximum bending moment at the tensile side 
- Rapid failure occurred when part could no longer withstand bending moment applied
![[Pasted image 20251103181336.png]]

## Characterization of Fatigue Loading 
- Stress ratio: $R = \frac{\sigma_{min}}{\sigma_{max}}$
- Mean stress $\sigma_{m} = \frac{\sigma_{max} + \sigma_{min}}{2}$
- Stress amplitude $\frac{\sigma_{max} - \sigma_{min}}{2}$
- Both the stress amplitude and mean stress values are used to estimate fatigue lifetime 
- Stress amplitude(S) is the value normally put on a cruve vs. estimated lifetime in number of cycles (N)

![[Pasted image 20251103182033.png]]
![[Pasted image 20251103182057.png]]
$\sigma_{min} = -\sigma_{max}$


## Fatigue lifetime from S-N Curves 
**This is the most important quantitative part of the Fatigue Lifetime Analysis**

SN Curve (Stress Amplitude vs. Cycles to Failure)
- Can use S-N curves to predict operation lifetimes of components given an applied stress amplitude 
- Decreasing stress amplitude leads to increase in service life 
- Can define fatigue strength value as being the stress amplitude level for failure to occur in a specified number of cycles 
- Can define fatigue strength value as being the stress amplitude level for failure to occur in a specified number of cycles 
- Fatigue limit (or endurance limit) is the stress amplitude below which fatigue failure does not occur 
- Not all materials have a fatigue limit. For example, steel has a fatigue limit, for example, steel has a fatigue limit, but aluminum does not (ie. aluminum may experience fatigue failure at even very small applied stresses, given large enough number of cycles)
![[Pasted image 20251103182814.png]]

## Fatigue 
![[Pasted image 20251103182918.png]]

## Plot showing varying probability of failure, P
![[Pasted image 20251103183011.png]]
- There can be considerable variability in fatigue behavior, especially in the imitation phase. This is related to population of defects. 
- A different curve can be used to express the probability of failure at a given stress. 

## Fatigue failure - some key points 
- Tensile stress required - will not form there are only compressive stresses present 
- Failure occurs when crack grows to critical length - when $K_{c}$ is exceeded (for a given nominal stress)

- Crack nucleation and growth: 
	- Cracks start at stress concentration points, usually on surface - dents, scratches, sharp filets, key-ways, machined threads etc. 
	- Can nucleate internally on microstructure discontinuities - porosity, inclusions etc. 
	- Cracks usually nucleate in the first 10-20% of service life. 
- Significant time is associated with initiation and crack growth 
	- This way, cracks can be found by inspection of structures: This is why we design structures with critical crack lengths that are large and easily detectable 
	- Internal fatigue cracks can be a lot more difficult to detect

### Visual inspection of fatigue failure 
- Striations - microscale features indicating crack origin and growth characteristics. One striation per load cycle. Not normally visible by eye, only by microscope or similar 
- Beachmarks - macro-scale feature visible by the eye. Usually associated with periodic start up and shut down of operation. These do not reflect load cycles. May have many striations within one beach mark. 
![[Pasted image 20251103183627.png]]

### Mechanism of fatigue crack growth due to fluctuating stress values 
![[Pasted image 20251103183653.png]]
a. unloaded crack 
b. tension applied, and local stress at sharp areas likely exceeds yield strength / UTS 
c. The crack tip yields and deforms to blunt the area locally, which reduces local stresses below yield strength / UTS 
d. e. f. Unloading reverse yielding and crack extension, repeat cycle.  

## Three Factors which affect Fatigue Life 
1. Mean stress (higher mean stresses have shorter lifetimes)
2. Surface treatments (introduce compressive stresses to reduce chance of crack initiation)
3. Design factors (reduce stress concentration areas)

**Mean Stress**
- As mean stress increases, the fatigue lifetime decreases. Higher mean stresses shift the S-N Curve downwards (fails at shorter cycle and lower stress amplitudes)
- ![[Pasted image 20251103200012.png]]
- If the mean is lower, and they end at the same cycle, is means that the amplitude of the stress must be higher, so the magnitude must be higher. 
- ![[Pasted image 20251103201017.png]]
- ![[Pasted image 20251103225018.png]]

**Surface Treatments**
- Treat the surface to reduce the formation and initiation of cracks on the surface. 
- Make the surface harder and tougher (case hardening)
- Introduce compressive stresses to the surface to prevent tensile forces to grow cracks (shot peening)
- ![[Pasted image 20251103232921.png]]

**Case Hardening**
- A hardened layer is created through atomic diffusion by exposing the metal to a carbon or nitrogen rich atmosphere at a high temperature (carburizing if done in carbon monoxide)
- Case or surface hardening hardens the surface of a metal object while allowing the metal underneath to remain soft, so the material maintains its toughness, but has a harder more wear-resistant surface. 

**Shot Peening**
- Residual compressive stresses are generated on the surface of a metal by shooting hard particles at it 
- Residual compressive stresses delay crack initiation (may retard crack growth)
- May also do this with lasers producing plasmas on the surface.

**Design Factors**
- reduce stress concentration areas. 