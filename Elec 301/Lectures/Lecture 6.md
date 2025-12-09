## Unilateral Laplace Transform 
![[Pasted image 20251205003825.png]]
- Applications: 
	- solving linear differential equations with constant coefficients 
	- Solving electrical circuits (transfer functions) by mapping them into s-domain. 

## Laplace transform as Operation Calculus. 
- Technique for solving linear systems 
- First developed on a large scale by Oliver Heaviside as a collection of rules (operation calculus). 

## Linear differential equations with initial conditions - linear circuits. 
- Differential operators transformed into algebraic operations in complex domain. 
- Initial conditions explicitly incorporated into solution
- ![[Pasted image 20251205004253.png]]

## Linear Differential Equations
- Physical LTI systems can be reduced to differential equation (implicit dynamics description of input output relation)
- ![[Pasted image 20251205010216.png]]

#### Initial Value theorem 
![[Pasted image 20251205010235.png]]
#### Final Value Theorem 
![[Pasted image 20251205010242.png]]

## Linear Differential Equations
- Clear separation between the effects of initial conditions and input signal. 
- The effects of ICs, for a stable system attenuates exponentially in time, disappearing in the steady state solution. 
- The transfer function ignores the initial conditions. 
- ![[Pasted image 20251205010614.png]]

## Poles and Zeros 
- Laplace transform links the transfer function description with the implicit descriptions as a dynamical system. 
- Transfer function for LTI system is characterized in the form of rational functions. 
- ![[Pasted image 20251205010721.png]]
- Zeros of numerator: zeros
- Zeros of denominator: poles 
- Location of poles and zeros in s-plane specifies H(s) up to a constant gain factor. 

## S-Plane Poles and Zeros 
- $s = \sigma + j\omega$
- $\sigma$ is the steady state solution (damping factor)
- $j \omega$ is the oscillatory frequency solution. 
- ![[Pasted image 20251205011006.png]]

## Solving Circuits using Laplace Transform 
- Convert circuit from time domain to Laplace s-domain. 
- IC becomes equivalent voltage / current sources 
- Solve the circuit to find required output Y(s) such that we would for a DC network. 
- Once Y(s) is determined, compute y(t) through inverse Laplace transforms. 
![[Pasted image 20251205011259.png]]

## Uses of Laplace Transform 
![[Pasted image 20251205011315.png]]

## Frequency Response - Bode Plots 
- Steady-state response of a system to a sinusoidal input test signal (no IC). 
- Frequency response is the transfer function H(s) when s = $j \omega$ . 
- Bode plots - display the log-magnitude and phase of H(s) vs $\log(\omega)$
- Bode plot is a graphical approximation technique in spectral domain. 
- System bandwidth concept. 
- ![[Pasted image 20251205011707.png]]


## Frequency Response Methods 
- Developed by Nyquist and Bode in 1930s. 
- Advantage in system design and analysis: 
	- Modeling transfer functions from physical data 
	- Finding stability conditions and stability margins 
	- Designing compensator networks to shape the desired response.

## Frequency Response 
- Frequency response - steady state response to a sinusoidal input signal. 
- Harmonic inputs to an LTI system generate harmonic response at the same frequency, but with differences in amplitude and phase angle from the input. 
- ![[Pasted image 20251205011924.png]]

## Bode Plots:
- Transfer function: tells us the response of the circuit at specific frequencies. 
- Zeros: increase rate of 20dB/dec. 
- Poles: decrease rate by 20dB/dec/

## Phase Plots 
![[Pasted image 20251205012930.png]]
- at $\omega = \omega_{p}$ the contribution is exactly 45 degrees. 
- Poles: Phase increases with 45 degree/decade. 
	- Net change + 90 degrees
- Zeros: Phase decreases with -45 degree / decade 
	- Net change - 90 degrees. 

## Constant Term contribution 
- Magnitude plot: Constant gain K contributes with a straight horizontal line of magnitude $20\log_{10}K$
- Phase plot - zero phase contribution. or 180$\degree$ if K<0. 

## Zero at Origin: 
$H(s) = s \implies H(j \omega) = j \omega$.
- This is the ideal deriver. 
- Magnitude plot: positive slope line with +20 db/Dec 
- Phase plot: +90 deg phase shift for each zero. 

## Pole at origin 
$H(s) = \frac{1}{s} \implies H(j \omega) = \frac{1}{j \omega}$
- This is the ideal integrator. 
- Magnitude plot: line passes through $\omega = 1$ with slope -20 dB /dec. 
- Phase plot is -90 degree phase shift. 

## Effect of Poles and Zeros on Phase 
- The effects are localized in the range of 1 decade in each the positive and negative direction 
	- $0.1 \omega < \omega_{p} < 10 \omega$

![[Pasted image 20251205013902.png]]
![[Pasted image 20251205013911.png]]
## Bode Plot Approximation Errors 
- Bode techniques are visual asymptotic approximations of the real magnitude and phase plots. There are approximation errors. 
- Largest errors are at the critical frequency (approximately 3dB for simples zeros/poles)
## Phase Plot Approximation Errors: 
- Largest errors for the phase plots occur at 0.1$\omega_{crit}$ and $10 \omega_{crit}$.
- The error is approximately 6 degrees for a simple pole or zero. 

## Higher order poles/zeros 
- Asymptotic trend: The power to which a pole or zero is raised by scales the rate of increase by that pole 
- $(s-\omega)^{n}\implies N*20 dB {dec}| N*90 \degree$
- Max errors at critical frequency increase for higher order poles / zeros, but asymptotic convergence remains. 