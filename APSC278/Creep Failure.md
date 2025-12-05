## Definition 
- Creep is a time dependent and permanent deformation of materials subjected to a constant load at elevated temperatures 
- For metals, creep is observed at temperatures above 0.4$T_{m}$ 
- There are three regimes: 
	- Primary creep 
	- Secondary (steady state) creep 
	- Tertiary creep (rupture)
-  Secondary creep (stage II) is the most important stage from an engineering design standpoint, as it persists for the longest periodic 

## Deformation at Elevated Temperatures 
- There are three components of strain to consider 
### $\epsilon_{total} = \epsilon_{E} +\epsilon_{P}  + \epsilon_{C}$
- $\epsilon_{E}$ instantaneous elastic strain 
- $\epsilon_{P}$ instantaneous plastic strain (when loaded above yield point)
- $\epsilon_{C}$ time-dependent creep strain 

## Typical Creep Test - Strain vs. Time under an applied static load 
![[Pasted image 20251104120824.png]]

## Three Stages of Creep 
1. Primary or instantaneous creep 
	- Continuously decreasing creep rate due to strain hardening 
2. Secondary or Steady state creep
	- Constant creep rate $\epsilon_{S}$ due to balancing of strain hardening and recovery; longest stage. 
3. Tertiary creep: 
	- Acceleration of creep rate until rupture at $t_{r}$
![[Pasted image 20251104121056.png]]

## Effect of Stress and Temperature on Creep 
![[Pasted image 20251104121222.png]]
- higher temperature implies faster/larger creep
- They are all the same material, and all factors other than temperature are different.  (includes applied stress)

![[Pasted image 20251104121529.png]]
- everything held constant - other than stress, whichever one fails first must have the highest operating stress. 

## Effect of stress and temperature on secondary (Stage II) Creep 
**Power Law Creep rate:**
$$\epsilon{s} = K_{2} \sigma ^{n} e ^ {\frac{-Q_{c}}{RT}}$$
- $\epsilon_{s}$ is the creep strain in the secondary regime 
- n is the creep exponent (material constant)
- $\sigma$is the stress (tensile or compressive)
- $Q_{c}$ is the activation energy for creep 
- $K_{2}$ is a material constant 
- R is the gas constant (8.31 J mol / K)
![[Pasted image 20251104122050.png]]

## Design Example $\textemdash$ Predicted Rupture Time 
- You may be given a graph of Stresses vs. Predicted Rupture Times 
- As stress or T increase, the rupture life decreases 
- Concept of 'rupture life'
	- Plot $\log(\sigma)$ vs. $\log(\text{rupture life})$
![[Pasted image 20251104122407.png]]

## Creep Rupture Time Estimates with Larson-Miller Parameters (same stress, vary temperature)
- Often need data for timescales impractical in lab experiments. One option is to perform tests at the same applied stress, but higher temperatures (thus shorter times) and extrapolate 
- A common procedure is the Larson-Miller parameter 

$$m = T(C+\log(t_{r}))$$
- C is a constant (approximately 20)
- T is the operating temperature of the component in K 
- $t_{r}$ is the estimated stress-induced rupture time (in hours)
- In general, the rupture lifetime of a material at some specific stress level will vary with T such that this m parameter stays constant
![[Pasted image 20251104123814.png]]


## All proposed mechanisms for creep behavior involve the different types of imperfections 
Several theoretical mechanisms have been proposed to explain the creep behavior for various materials: these mechanisms involve: 
- stress-induced vacancy diffusion 
- grain boundary diffusion 
- dislocation motion 
- grain boundary sliding 
Each leads to a different value of the stress exponent n in the power law relation. It has been possible to elucidate the creep mechanism for a particular material by comparing its experimental n value with values predicted for the various mechanisms 

## Mitigate creep by increasing grain sizes 
- Grain boundaries play an important role at higher temperature deformation 
	- To obtain the very best creep resistance, one can reduce the number of grain boundaries (increase grain size)
	- The ultimate prevention to creep is to have a single crystal. This is true even with the decrease in yield strength and ultimate tensile strength 
- For example, high performance turbine blades operate in a high-stress, high-temperature environment, and are made from single crystal Ni superalloys 

