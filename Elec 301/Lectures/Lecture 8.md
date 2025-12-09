## Band Pass Circuits 
- Note the bode straight-line approximation is best when quality factor is low. 
- $Q = \frac{1}{2\tau}$
	- If Q is high, there is very low damping, so the real magnitude bends far away from the straight-line bode asymptote. 
	- If Q is low, the frequency response is smoothed. 
	- Curve follows asymptotic bode lines closely.

### Conclusions: 
- Bode plots give a fast visual approximation technique for the frequency response of a linear system. 
- It provides a good approximation for systems with distinct individual poles on the real axis. 
- The typical cases for electronic amplifiers correspond to such a situation. 

## Simple LP active amplifier
- simple low pass amplifier stage with passive feedback network. 
- ![[Pasted image 20251206130521.png]]
- The fundamental principle of negative feedback is that it decreases gain but increases bandwidth. 
- So, Critical frequency is increased by the loop gain. 
- If the amplifier is a high pass filter, the negative feedback will decrease the low-cutoff critical frequency.

## Open-Circuit/Short-Circuit time constant method. 
- Low frequencies: 
	- Impedance of low pass filters is very high. Treat as an open circuit 
- High frequencies:
	- Impedance of high pass filters is very low, treat as short circuit. 

### Band Pass Filter: 
![[Pasted image 20251206131545.png]]
### Low Frequency: 
![[Pasted image 20251206131625.png]]

#### Determining the Poles at low frequency: 
$\left( \frac{V_{0}}{Vi} \right)= \frac{R_{2}}{R_{2}+R_{1}+\frac{1}{sC_{1}}} = \frac{sR_{2}C_{1}}{1+s(R_{1}+R_{2})C_{1}}$
Then, converting to frequency domain, to find the pole, we examine the zero of the denominator. 

$\omega_{p_{1}} = \frac{1}{(R_{1}+R_{2})C_{1}}$

### High Frequency: 

![[Pasted image 20251206131644.png]]

#### Determining the poles at high frequency: 

$\frac{V_{0}}{V_{i}} = \frac{R_{2} || / \frac{1}{sC_{2}}}{R_{1} + R_{2}|| \frac{1}{sC_{2}}} = \frac{R_{2}}{R_{1}+R_{2}+sR_{1}R_{2}C_{2}}$

$\frac{V_{0}}{V_{i}} = \frac{R_{2}}{R_{1}+R_{2}} \frac{1}{1+sC_{2}\left( \frac{1}{R_{1}} + \frac{1}{R_{2}} \right)^{-1}}$

Determining the pole by converting to frequency domain and solving for the zero of the denominator. 

$\omega_{p_{2}} = \frac{1}{R_{1}||R_{2}C_{2}}$


Based on this, we can combine the midband, the low frequency and the high frequency. 

$H(s) = \frac{R_{1}}{R_{1}+R_{2}} \cdot \frac{s}{s+\omega_{p_{1}}}\cdot \frac{w_{p_{2}}}{s+\omega_{p_{2}}}$

When we move to the bode plot for, we want everything to be normalized, this is where the zero comes from, as well as the frequency in the numerator. 

## Generalization: 
- We can in general, provided that the poles groups are apart, separate an amplifier frequency response into the three bands components:: 
- $T(s) = A_{m} F_{L}(s)F_{h}(s)$
	- $A_{m}$ is the midband
	- $F_{l}$ is the low frequency (high pass)
	- $F_{h}$ is the high frequency - low pass. 
![[Pasted image 20251206134655.png]]
![[Pasted image 20251206134706.png]]


## Generic amplifier frequency response. 
- Bandwidth (BW) = $\omega_{H_{3db} - \omega_{L_{3db}}}$
- ![[Pasted image 20251206134829.png]]


## Finding the cut-off frequency $\omega_{L_{3dB}}$
Solve the following equations once knowing the transfer function (poles and zeros are known)
$|F(j\omega_{L_{3dB}})|^{2} = \frac{1}{2}$
- Since the low cutoff frequency is larger than any pole or zero, we can approximate the equation using only the highest powers. 
- $\omega_{L_{3dB}} = \sqrt{\omega_{Lp_{1}}^{2}+ \dots \omega_{LpN}^{2} -2\omega_{Lz_{1}}^{2} - \dots - 2\omega_{Lz_{n}}^2}$

## Finding the cut-off frequency $\omega_{3dbH}$.
- Since M>m and $\omega_{3dBh}$ is smaller than any of the pole or zero frequencies, we can approximate the equality considering only the dominant low powers in the polynomials. 
- $\tau_{H_{3db}} = \sqrt{\tau_{Hp_{1}}^{2}+ \dots \tau_{Hp_{2}}^{2} - 2\tau_{Hz_{1}}^{2}-2\tau_{Hzm}^2}$
