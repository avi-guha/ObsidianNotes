# BJT Small Signal Model 

## Amplifiers with BJT 
- Small signal model (hybrid $\pi$ model)
- Notation conventions 
	- Capitals denote DC values
	- Lower case subscripts on capitals denote complex values (phasors)
	- Lowercase I's and v's with capital subscripts denote instantaneous values 
	- Lowercase i's and v's with lowercase subscripts denote small signal values. 

## Diodes
- Three operating regions 
	- Forward-bias region 
		- ($V_{D} >0$)
	- Reverse bias region 
		- $V_{BR} < V_{D} < 0$
	- Breakdown region
		- $V_{D} < V_{Br}$
- Diode law (Shockley diode)
	- $I_{s}$ - reverse bias saturation current 
	- n = ideality factor (1.2)
	- $V_{t} = \frac{kT}{q}$
	- $i_{D} = I_{s}(e^\frac{v_{D}}{nV_{t}}-1)$

## Biasing - Quiescent point 
- DC level biasing - ($V_{D}, I_{D}$) is called the quiescent point. 
- $V_{D} = V_{T}\ln\left( \frac{I_{D}}{I_{s}} \right) \approx_{} 0.6 \text{~} 0.7$
- ![[Pasted image 20251206162714.png]]

Diode small signal model 
- DC + small AC voltages. 
- nonlinear circuit implies we cannot use superposition. 
- $I_{D}+i_{d} \approxeq \frac{V_{1}-0.6V}{R} + \frac{v_{i}}{R+r_{d}}$
- ![[Pasted image 20251206163318.png]]


## Bipolar junction transistors (BJT)
- Two types: npn, pnp
- Three terminals: emitter, base, collector. 

## BJT operating Modes 
- Saturation - transistor is like a short circuit and current freely flows from collector to emitter 
- Cut-off - transistor acts like open circuit, no current flows from collector to emitter 
- Active - Current from collector to emitter is proportional to base current 
- Inverse-Active - current proportional to active mode, but flows in reverse from emitter to collector. 
![[Pasted image 20251206171530.png]]

## BJT in active operating mode 
- EB junction must be forward viased ($V_{be} is 0.6V$)
- CB junction is reverse biased $V_{cb}$ >0
- DC Model: 
	- $i_{C} = I_{C} + i_{c}$
	- $I_{C}= \alpha I_{E} \beta I_{B}$
	- $I_{C}=I_{s}e^{V_{BE}/V_{T}}$
	- alpha is common base current gain factor 
	- beta is common emitter current gain factor 
	- Thermal voltage is the thermal energy available to charge carriers (25mV at 25 degrees)
	- Is is the reverse saturation current. 
- ![[Pasted image 20251206171833.png]]


## Large Signal model of BJT 
- REM: Coupled junctions
- ![[Pasted image 20251206173711.png]]

## BJT Parameters: 
- Saturation current: $I_{s} < 10^{-16} A$
- Thermal voltage $V_{T} = \frac{kt}{q}$ = 25mV
- Forward current gain $\beta_{f} = \beta$ assumed constant 
- NPN: 50-500
- PNP: 10-100

## BJT current-voltage relations

- Early effect (discovered by James Early)
- Early voltage $V_{A}$ models the early effect 
- $V_{A}$ = 50-100V (larger better)
- ![[Pasted image 20251206174113.png]]

## Small variations around the DC bias values 
- Consider a small variation around the DC bias value 
- $v_{BE} = V_{be} + V_{BE}$
- $i_{C} = I_{S}e^{\frac{V_{BE}}{V_{T}}}=I_{S} e^{V_{BE}/V_{T}}e^{\frac{v_{be}}{V_{T}}}=I_{c}e^{v_{be}/V_{T}}$
- $i_{C} = I_{C}+i_{c} \approx I_{C}(1+\frac{v_{be}}{V_{t}})$
- $i_{c} = \frac{I_{c}}{V_{T}}v_{be}$
- $g_{m}$ is the small signal transconductance. 

## Transconductance $g_{m}$
$g_{m}$ is the slope of the $i_{C}(v_{BE})$ curve.
![[Pasted image 20251206174813.png]]

## Base-emitter input port 
$i_{B}=I_{B}+i_{b}= \frac{i_{c}}{\beta} = \frac{I_{C} + i_{c}}{\beta} = \frac{I_{C}}{\beta} + \frac{g_{m}}{\beta}v_{be} \implies i_{b}=\frac{g_{m}}{\beta}v_{be}$
- small signal input resistance 
- $r_{\pi} = \frac{v_{be}}{i_{be}}=\frac{\beta}{g_{m}}=\frac{\beta V_{t}}{I_{c}} = \frac{V_{T}}{I_{B}}$

## Basic Hybrid-Pi model for BJT 
- Small signal model without considering capacitive effects nor the early effect. 
- ![[Pasted image 20251206175213.png]]
- 