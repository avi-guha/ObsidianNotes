---
~
---

# Stress Concentrations

### Defects can lead to stress concentrations and eventual fracture 
- All manufactured materials contain defects 
	- Porosity 
	- Inclusions (contamination from processing) 
	- Surface damage accumulated during manufacturing or during service 
	- Dislocations 
	- Intentional 'defects' 
	- Defects can act as locations where local stresses can be much larger than the applied stress far away from defect. 
- ![[Pasted image 20251008132322.png]]
- This shows uniform stress distribution, which is not true. 
- Stress should concentrate at the tips $\textemdash$ this is known as stress concentration

### Stress concentration due to surface or internal cracks  
- $\sigma_{m} = \text{maximum local stress due to surface cracks or internal cracks}$ 
	- $\sigma_{m} = 2 \cdot \sigma_{o} \left( \frac{a}{p_{t}} \right) ^ {\left( \frac{1}{2} \right)}$ 
	- a $\textemdash$ length of surface crack, half the internal crack
	- $\sigma_{0}$ nominal stress 
	- $\sigma_{m}$ - maximum stress at crack tip
	- $\rho_{t}$ radius of curvature at tip 
$K_{t}$ = stress concentration factor for surface of internal cracks 
- $K_{t} = \frac{\sigma_{m}}{\sigma_{o}} = 2 \cdot\left( \frac{a}{\rho_{t}} \right) ^ {\frac{1}{2}}$
- With sharp cracks the radius of curvature approaches 0, implying that the stress at the crack tip approaches infinity
- In reality crack tips have a finite radius of curvature
- Mathematicians use the stress concentration factor, $K_{t}$ to study the stress fields at the crack tip.

### Theoretical $K_{T}$ values for other geometries.
![[Pasted image 20251008141749.png]]

### A note about calculating nominal stress - Solid Mech + MTRL
- In fracture calculations when calculating nominal stress we can either use full cross-sectional area including the crack cross section, or can subtract the crack cross-section from the cross section area. 
- If we use nominal stress it is okay to do either but good practice to specify. 
- ![[Pasted image 20251008142514.png]]


### Question 
![[Pasted image 20251008143128.png]]
Correct answer: C -- stress is maximized at tip. 

### Presence of Defects Affects overall strength of material specimens
- Ductile materials are less sensitive to stress raisers because they can yield at the crack tip the theoretical cohesive strength of a brittle elastic solid is not achieved because of the presence of microscopic Defects

## Fracture Toughness 

Fracture toughness is used to predict whether or not immediate catastrophic failure may occur for a specific geometry, material, and applied stress. 
- Stress concentration refers to crack tip radius of curvature
- Many cracks are very sharp $\rho \rightarrow 0$
	- Implies that the stress at crack tip is infinite if crack is infinitely sharp 
- Mathematicians look at stress distribution around crack to derive an alternat concept to analyze these stress distributions. 
- These factors combine all elements of fracture mechanics and material properties $\textemdash$ elastic and plastic deformation, toughness and other properties into one material property we call Fracture Toughness, $K_{c}$

### Fracture Toughness $K_{c}$
Fracture toughness $k_{c}$ is:
- a material property which estimates a material's resistance to brittle fracture in the presence of a crack
- Not the same as stress concentration factor (dif units)

## $K_{c} = Y \sigma_{c} \sqrt{\pi a}$ 
- Y is dimensionless, depends on geometry of crack and specimen as well as a manner of load application 
- $\sigma_{c}$ critical stress for failure
- a length of surface crack or half length internal crack 
![[Pasted image 20251008145216.png]]

## There are three modes of loading, each with associated fracture toughness $K_{c}$
![[Pasted image 20251008150853.png]]
- Mode I describes plain strain, in which the crack opens and there is no strain perpendicular to the front and back faces
- Plain strain fracture toughness is $K_{ic}$ and is often the most cited. It is the most common and important for structural components, representing the most severe condition 
- ![[Pasted image 20251008153342.png]]
- As yield strength increases, then plain strain fracture toughness decreases. 

### Fracture Toughness - dependence on Critical Stress for crack propagation and Crack Length
$K_{ic} = Y \sigma_{c} \sqrt{\pi a}$ 
- The above expression can be rearranged to find 
	- The critical crack length ac, for a given applied stress. Cracks longer then $a_{c}$ will propagate and lead to fracture 
	- The critical stress $\sigma_{c}$ for a given crack length. Stresses above $\sigma_{c}$ will be large enough for the crack to propagate and lead to fracture 
	- ![[Pasted image 20251008155238.png]]
	- ![[Pasted image 20251008155253.png]]
	- ![[Pasted image 20251008155306.png]]

## Procedure to estimate whether the crack will catastrophically propagate 
1. Determine the length, crack geometry and mode of loading 
2. Calculate the fracture toughness, $K_{c}$ for the geometry and load 
3. Look up fracture toughness value, $K_{ic}$ of the material from a handbook 
4. Check if $K_{C} \geq K_{ic}$
	1. Yes then rapid crack propagation takes place 
	2. No, then check if other modes of failure can occur 

![[Pasted image 20251008174220.png]]

Example Problem: 
![[Pasted image 20251008174300.png]]
![[Pasted image 20251008174314.png]]
![[Pasted image 20251008174357.png]]
![[Pasted image 20251008174410.png]]

### Discussion: 
- A 17mm critical crack in a 100mm x 20mm is quite large. So this implies that the alloy is quite strong 
	- The material is often used in transport application, including marine, automotive and aviation.
- The crack length seems large than a crack due to a manufacturing related defects or an in-service scrape or scratch. 
- A crack of this length is detectable prior to failure, which is desirable. 

## Detecting cracks before they become critical is desirable
- If detected before failure, we can remove the component from service. 
- We can use a variety of methods to determine if this crack exists 
	- Visual inspection
	- Non-destructive testing 
	- Die penetrant
	- Ultrasonic methods
	- Use concepts such as leak before break in pressure vessels 

## Example 2
![[Pasted image 20251103180713.png]]


## Breaking conditions
- Break before leak 
	- Critical crack size value is smaller than wall thickness - can catastrophically break through 
- Leak before break 
	- Critical crack size value is larger than the thickness of the vessel walls
- 