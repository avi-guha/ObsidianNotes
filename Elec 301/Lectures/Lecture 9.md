# Open Circuit Short Circuit Time Constant methods 

## Bandwidth Estimation 

### Standard estimation:
- Derive T(s)
- set $s = j \omega$
- Find $|T(j \omega)$
- Set the magnitude lower by -3db of the 'midband' value.
- Solve for $\omega_{L_{3dB}}$ and $\omega_{H_{3dB}}$
### Difficulties: 
- difficult computation by hand 
- We desire design insight - which components et the BW limits, this is approximative, but with insight. 

## Method of OC and SC time constants 
- OC - open circuit 
- SC - short circuit. 
- How to estimate the cut-off frequencies when the locations of poles and zeros are not known. 
- The approximation is reasonably accurate provided that the next nearest poles, are at least two octaves (4x) away, or at least a decade away. 
- Design aspects: OCT and SCT, unlike typical circuit simulations can identify which elements are responsible for BW limitations. 

## Short Circuit. The cut-off frequency $\omega_{L_{3db}}$
- We focus on $F_{l}(s) = \frac{s^{n}+ a_{1}s^{n-1} + \dots +a_{n}}{s^{N}+b_{1}s^{N-1}+\dots b_{N}}$
$$b_{1} = \omega_{LP_{1}} + \omega_{LP_{2}} +\dots \omega_{LP_{N}} = \frac{1}{\tau_{c_{1}}} + \frac{1}{\tau_{c_{2}}} + \dots \frac{1}{\tau_{C_{N}}} = \sum^{i=1}_{N} = \frac{1}{C_{i}R_{is}}$$

- $\tau_{sc}$ is the short circuit time constants. Each $R_{is}$ is the resistance seen by the i'th low frequency capacitor, with all other capacitors replaced by short circuits. 
- ALL high frequency capacitors are short circuits. 

## Finding the cut-off frequency $\omega_{L_{3dB}}$
- If one of the pole frequencies (or one of the $\frac{1}{\tau^{sc}}$) or zero frequencies by a factor of 10x. Then Fl(s) can be approximated for frequencies above those of other poles and zeros. 
- $F_{l}(s) = \frac{s}{s+b_{1}}$
- $\omega_{L_{3dB}} = b_{1}$

## Open Circuit, finding the cutoff frequency $\omega_{3dBH}$

- focus on $F_{H}(s)$
- $F_{H}(s) = \frac{1}{H_{p_{1}}} + \frac{1}{H_{p_{2}}} \dots \frac{1}{H_{p_{N}}} = \tau_{C_{1}} + \tau_{C_{2}} + \tau_{C_{3}} = \sum^{i=1}_{M}C_{i}R_{io}$.
- Here the time constants are 'open circuit' time constants. Each RIO is the equivalent resistance as seen by the i'th HF capacitor. 
- All other HF capacitors are open circuits.
- All the LF capacitors become short circuits. 

### Finding the cutoff frequency $\omega_{3dBH}$
- If one of the pole frequencies is larger than the others by 4x, or even better 10xm we can approximate the $F_{H}(s) = \frac{1}{1+d_{1}s}$  
- $\omega_{H_{3dB}} = \frac{1}{\sum^{i=1}_{M}C_{i}R_{io}}$

## Remarks 
- For our initial circuit below
- ![[Pasted image 20251206160628.png]]
- At low frequencies:
	- $C_{1}$ acts as a high pass filter; at low frequencies, it has not yet activated so C2 is an open circuit.
- At high frequencies:
	- C2 acts as a low pass filter, C1 will have next to no impedance so we treat it as a short circuit. 
