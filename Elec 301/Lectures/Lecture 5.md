
## General Analysis Principle 
- Decompose input signal into elementary components. 
- Characterize the system response to the elementary signals. 
- Use superposition and time invariance to reconstruct the output from the responses to elementary components. 

## System modelling and analysis in the time domain. 
- LTI systems are completely characterized by their unit impulse response. 

## Time domain analysis of LTI systems. 
- The response of a given LTI system to unit impulse is enough in order to know everything about the system behavior. 
- Principle: Decompose signals in terms of Dirac/Unit Impulses. 
![[Pasted image 20251205073946.png]]
## LTI description 
- Time-domain description - based on representing signals as linear combinations of shifted impulses. 
- Easy signal decomposition, but the unit impulse changes its shape in propagating through a system. 
- because of linearity, any linear decomposition of the input signals in terms of a set of basic signals will give an output as a linear combination of the system's responses. 

## Frequency-domain analysis of LTI systems - Fourier Transform.
![[Pasted image 20251204234749.png]]

$X(t)$ and $X(j \omega)$ are called a Fourier transform pair. $j \omega$ is called the frequency domain representation of $x(t)$.


## Key points 
- The importance of exponential and harmonic signals 
	- LTI systems - signals propagate without changing shape. 
	- In nature - many natural sensors operate based on harmonic decomposition 
	- Periodic signals, attention discrete time signals. 
	- Need at least second order DE to generate oscillations.

## Interpretation of Frequency Response. 
- Sinusoidal steady state response: 
	- ![[Pasted image 20251204235333.png]]
	- $|H(\omega)|$ is the magnitude response of the system - shows the scaling of the input. 
	- $arg\{H(\omega)\}$ is the phase response of the system. 
		- Shows the frequency dependent delay in propagating a sinusoid through the system.
![[Pasted image 20251204235559.png]]


## Unilateral Laplace Transform 
- Laplace transform is an extension of the Fourier Transform. 
- The real portion tells us the dampening of the system over time. 
- This is why the Laplace transform does not preserve the energy of the signal in the spectral domain. Numerical inversion of Laplace transform is difficult. 
- LT lets us use operation calculus. Maps circuits from time domain to s domain. 
- $\frac{d}{dt} \rightarrow s$ and $\int dt \rightarrow \frac{1}{s}$

- We assume the system is causal (only depends on present and pass). So we can work with initial conditions. 
- The causality assumption removes the abiguity existent in the bilateral LT
- Main applications: analysis of linear electrical circuits, or system described by differential equations with initial conditions. 
![[Pasted image 20251205000423.png]]

## Remarks
$$X(s) = \int^{\infty}_{0} x(t)e^{-st}dt$$
- The lower 0 limit means that we do include in the integral discontinuities and impulses that occur at t=0. 
- The inverse LT is generally computed not by using contour integration in complex plane, but by combining properties with partial-fraction expansion. 
- Convergence: for a signal x(t) to have Laplace transform, it is sufficient to satisfy $\int^{\infty}_{0}e^{-\sigma t}dt < \infty$

## Fourier vs. Laplace Transform
![[Pasted image 20251205002108.png]]
- Energy preservation: only Ft preserves the energy (orthogonal transform) easier to numerically compute both the direct and inverse FT (good numerical stability).
- Class of signals: Lt allowed for an extended class of (causal) signals. 
- Linearity: both Ft and LT are linear operators 
- Transformations on calculus operators ($\frac{d}{dt}$): mapping to algebraic operations ($j \omega$) or $\frac{1}{j \omega}$
- Initial conditions: explicitly kept in LT.

## LT Properties 
![[Pasted image 20251205002545.png]]

## Transfer Function 
- In the case of a general dynamical system description (implicit characterization through a system of differential equations)
- ![[Pasted image 20251205002656.png]]
- The input-output transfer function is obtained when all ICs are set to zero. 
- ![[Pasted image 20251205002706.png]]



 