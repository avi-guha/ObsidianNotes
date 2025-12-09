## Example 1: Low pass filters

$$H(s) = \frac{1}{2s + 100} = \frac{1}{100} \frac{1}{1+\frac{s}{50}}$$
Converting to the frequency domain, we have:

$H(s) = \frac{1}{100} \frac{1}{1+j \frac{\omega}{50}}$
This is a low pass filter. There is a pole at 50 rad/s

The midband gain is: 
$H_{DC} = 0.01 \implies 20\log_{10}(0.001) = -40dB$
![[Pasted image 20251206125612.png]]

## Phase Plot Approximation Errors: 
- Largest errors for the phase plots occur at 0.1$\omega_{crit}$ and $10 \omega_{crit}$.
- The error is approximately 6 degrees for a simple pole or zero. 

## Higher order poles/zeros 
- Asymptotic trend: The power to which a pole or zero is raised by scales the rate of increase by that pole 
- $(s-\omega)^{n}\implies N*20 dB {dec}| N*90 \degree$
- Max errors at critical frequency increase for higher order poles / zeros, but asymptotic convergence remains. 

