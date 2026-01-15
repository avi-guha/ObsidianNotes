
## Problem 1: Multi-transistor Amplifier Circuit. 

In this problem, we tackle a two stage amplifier, that ahs a common-emitter stage and a commo-base stage. The following circuit is known as a cascode amplifier. ![[Pasted image 20251209144839.png]]

For this problem, we use the 2N3904 model of the BJT. And below are the recommended minimum specifications for the transistor: 
![[Pasted image 20251209144930.png]]

We also assume the following characteristics: 
- $R_{s} = 50  \Omega$
- $V_{cc} = 20V$
- $R_{L} = 50k\Omega$

### Finding the resistor values to bias the transistors. 
We can do this using the $\frac{1}{4}$ rule and $\beta=167$, which are commonly accepted values for the BJT: 2N3904.

$$V_{c_{1}} = 15V \qquad V_{E_{1}} = V_{C_{2}}=10V \qquad V_{E_{2}} = 5V \qquad V_{B_{1}} = 10.7V \qquad V_{B_{2}}=5.7V$$

$I_{C_{1}} \approx I_{E_{1}} \approx I_{E_{2}} = \frac{V_{cc} - V_{c}}{R_{c}} = \frac{5V}{2.5k \Omega} = 2mA$
$I_{B_{1}} \approx I_{B_{2}} = \frac{I_{c}}{\beta} = \frac{2mA}{167V} = 11.98 \mu A$

$I_{1} = \frac{I_{E}}{\sqrt{ \beta }} = \frac{2mA}{\sqrt{ 166.67 }} = 154.76 \mu A$
$I_{2}=I_{1}-I_{B_{1}} = 154.76 \mu A - 11.98 \mu A = 142.79 \mu A$

Now we can calculate exact resistor values. 
$R_{B_{1}} = \frac{V_{cc}-V_{b_{1}}}{I_{1}} = 60.1k \Omega$
$R_{B_{2}} = \frac{V_{B_{1}} -V_{B_{2}}}{I_{2}} = 36k \Omega$
$R_{B_{3}} = \frac{V_{B_{2}}}{I_{3}} = 42.6k \Omega$
$g_{m} = \frac{I_{c}}{V_{T}} = 0.08S$
$r_{\pi} = \frac{\beta}{g_{m}} = 2.09 k \Omega$
Based on our exact resistor values, we can specify the more commonly used resistances. 

$R_{B_{1}} = 62k \Omega$
$R_{B_{2}} = 36 k \Omega$
$R_{B_{3}} = 43k \Omega$

To satisfy the minimum input resistance of $R_{in} = 3.5 k \Omega$, we can add a resistance to the base. 
$R_{in}=3.5k \Omega = R_{B_{2}} || R_{B_{3}} + r_{\pi} +R_{add} \rightarrow 1.6k \Omega$
Specifying a common resistance: $R_{add} = 2k \Omega$

Just to refresh, the small signal parameters of the 2N3904 transistor are:
$r_{\pi} = 2.09k \Omega$
$g_{m} = 0.08S$

| Transistor          | \(V_C\)  | \(V_B\)   | \(V_E\)   | \(I_C\)    | \(I_B\)     | \(I_E\)    |
|--------------------|----------|-----------|-----------|------------|-------------|------------|
| Top Transistor (Q2) | 15.4 V  | 10.4 V    | 9.75 V    | 1.96 mA    | 11.66 µA     | 1.97 mA    |
| Bot Transistor (Q1) | 9.75 V  | 5.38 V    | 4.71 V    | 1.97 mA    | 11.72 µA     | 1.98 mA    |
Since we know the DC operating point of the circuit, we can move on to find the Bode plot frequency response of the circuit. 

$w_{C_{E}}^{SC} = \frac{1}{\frac{C_{E}*(R_{B_{2}} || R_{B_{3}}||(R_{S}+R_{add}))}{1+\beta}}||R_{E} \approx \frac{1}{23.25*C_{e}}$
$\omega_{Cc_{2}}^{SC} = \frac{1}{C_{C_{2}}*(R_{C} +R_{L})} \approx \frac{1}{52k \Omega * C_{C_{2}}}$
$\omega_{C_{C_{1}}}^{SC} = \frac{1}{C_{C_{1}}*R_{B_{2}}||R_{B_{3}}||R_{\pi}+2K} = \frac{1}{4k\Omega*C_{C_{1}}}$

From these calculations, we can see that $C_{E}$ is responsible for the dominant pole of the bode frequency response plot. 

Our requirements state that the maximum $\omega_{L_{3dB}}$ frequency is 1200 rad/s. with this frequency, we can determine the corresponding value for the emitter capacitor. 

$\omega_{3DBL} = 1200 rad / s= \sqrt{ (23.249*c_{E})^{-2} -2*(2400*C_{E})^{-2} } \rightarrow C_{E} = 35.84 \mu F$. Rounding $C_{E} = 35.84F = 39 \mu F$. 

To keep the parts at a minimum, we can just set the two other collector capacitors to the same value as the emitter, $C_{C_{1}}=C_{C_{2}}=C_{E}$. Now, Recalculating the $\omega_{L_{3dBL}} = \sqrt{ (22.249*39\mu F)^{-2} -2(2400*39 \mu F)^{-2} } = 1103 rad / s= 175.5Hz$

Now, we can calculate the high frequency 3dB pole. 
$C_{\pi} = 2*C_{JE} + TF *g_{m} = 2*4.5pF + 400 pF*0.08 = 41pF$
$C_{\mu} = \frac{C_{JC}}{\left( 1+\frac{V_{CB}}{V_{JC}} \right)^{MJC}}= 1.79pF$

$\omega_{HP_{1}} = \frac{1}{(C_{\pi_{1}} + 2C_{\mu_{1}}(R_{s}+R_{add}||R_{BB}||R_{\pi_{1}}))} \approx \frac{1}{1k\Omega * 44pF}$
$\omega_{HP_{2}} = \frac{1}{C_{\mu}(R_{L}||R_{C})} = \frac{1}{12\Omega * 44pF} \approx \frac{1}{2.3k \Omega *1.8pF}$
$\omega_{HP_{3}=\frac{1}{C_{\mu}(R_{L}||R_{C}})}$
By inspection, we see pole 1 and 3 are dominating. 
So, Our cutoff high frequency is $\omega_{3dbL}=\sqrt{ (2.3k\Omega)^{2}+ (1k\Omega *44pF)^{2 }}= 3.6MHz$

Now, we can simulate the circuit below. Which is the small signal,  high frequency model of our cascode amplifier. 
![[Pasted image 20251209193321.png]]
 This is the bode plot from running an AC analysis: ![[Pasted image 20251209194245.png]]
The bode plot shows us a pole at cutoff frequency at approximately 3.6MHz, very similar to what our calculations showed as well. 

|             | $\omega_{3DBL}$ | $\omega_{3DBH}$ |
| ----------- | --------------- | --------------- |
| Calculation | 176 Hz          | 3.6 MHz         |
| Simulation  | 165 Hz          | 3.6 MHz         |
We can also cross reference without our full circuit simulation in LTSPice to show that the small signal frequency for high cutoff is nearly identical: 
![[Pasted image 20251209194726.png]]


![[Pasted image 20251209194711.png]]
### Part A: Simulating the DC Operating Point 
$A_{M} = \frac{V_{o}}{V_{s}} = \frac{V_{o}}{V_{\pi_{2}}} \times \frac{V_{\pi_{2}}}{V_{\pi_{1}}} \times \frac{v_{\pi_{1}}}{V_{s}} = -g_{m} \times (R_{L} || R_{C}) \times \frac{r_{\pi} || R_{B_{2}}||R_{B_{1}}}{r_{\pi}||R_{B_{2}}||R_{B_{1}}+R_{s} +R_{add}}= -78  \frac{v}{v}$ 

| Quantity  | Value       | Type           |
| --------- | ----------- | -------------- |
| V(n004)   | 5.76807     | voltage        |
| V(output) | 14.9385     | voltage        |
| V(n002)   | 10.8415     | voltage        |
| V(n003)   | 10.1675     | voltage        |
| V(n005)   | 5.09404     | voltage        |
| V(n001)   | 20          | voltage        |
| I(R8)     | 0.00210894  | device_current |
| I(R10)    | 0.00014093  | device_current |
| I(R11)    | 0.00212252  | device_current |
| Ic(Q3)    | 0.00211573  | device_current |
| Ib(Q3)    | 6.78896e-06 | device_current |
| Ie(Q3)    | -0.00212252 | device_current |
| Is(Q3)    | 0           | device_current |
| I(V3)     | -0.00225666 | device_current |
| I(R9)     | 0.000147717 | device_current |
| I(R12)    | 0.000134141 | device_current |
| Ic(Q4)    | 0.00210894  | device_current |
| Ib(Q4)    | 6.78686e-06 | device_current |
| Ie(Q4)    | -0.00211573 | device_current |
| Is(Q4)    | 0           | device_current |
| I(C1)     | 2.16831e-15 | device_current |
These values were determined by simulating the following circuit in LTSpice:
![[Pasted image 20251209171452.png]]



### Part B: Bode Plots for Magnitude and Phase. 
From before, we have the bode plot of the entire circuit, we can compare the simulated vs. calculated results. 
![[Pasted image 20251209194711.png]]
![[Pasted image 20251209194726.png]]
Our calculation and simulated results are very close, meaning the 1/4 method with OCSC time constants does work well when configuring and specifying components for the cascode amplifier. 

|             | $\omega_{3DBL}$ | $\omega_{3DBH}$ |
| ----------- | --------------- | --------------- |
| Calculation | 176 Hz          | 3.6 MHz         |
| Simulation  | 165 Hz          | 3.6 MHz         |

### Part C: Adjusting to see the linear vs. non-linear response of the amplifier. 

For this part of the problem, I fixed the frequency at 10kHz, then wrote a spice directive to adjust the amplitude of the signal from 1mV to 200mV. Then, Plotted the outputs $V_o$. Doing so showed me where the output became non-linear. From my simulation I found it to be at about 70mV
![[Pasted image 20251209200958.png]]

### Part D: Input impedance of the amplifier
Based on our simulations, we can compare the calculated vs. simulated impedances of the amplifier: 

**Measured:**
$R_{in }= R_{B_{2}}||R_{B_{3}}||R_{\pi} + R_{add}=3.69 k\Omega$
**Simulated**
$R_{in} = \frac{V_{in}}{I_{in}} = 4.01k \Omega$

**Discussion**
From our initial requirements, we see that both values do satisfy our minimum input resistance of 3.5$k\Omega$. Furthermore, since they are quite close in value we can conclude that the calculations and simulations were done correctly. 

## Question 2: Cascaded Amplifier

![[Pasted image 20251212140626.png]]

The circuit above can be used as a repeater in analog $50 \Omega$ coaxial cable system. Our task is to specify the components in the circuit such that the input impedance has a range of: $45 \Omega \leq R_{i} \leq 55 \Omega$, and the output impedance, $45 \Omega \leq 50 \leq 55 \Omega$. We also choose the same transistor as the previous problem, 2N3904, for the two transistors $Q_{1}$ and $Q_{2}$.
### Part A: Specifying components for the circuit based on requirements.

To start this problem, we choose $V_{cc} = 12V$ and use the $\frac{1}{3}$ rule such that $V_{E_{1}} = \frac{V_{CC}}{3}$. This will be useful in finding the resistances and capacitances of our circuit. We also round our values to the most commonly found values. 

$$V_{C_{1}} = 10V \qquad V_{B_{1}} = 5.7V \qquad V_{E_{1}} = 5V \qquad V_{C_{2}} = 15V \qquad V_{B_{2}} = 10V \qquad V_{E_{2}} = 9.3V$$

Using our $R_{in}$ requirements, we can create a system of equations. 

$R_{in} = 50 =R_{E}|| \frac{R_{\pi + 1}}{1+\beta}$
$R_{E_{1}} =\frac{V_{E}}{I_{E_{1}}}$
$R_{\pi} = \beta \times \frac{V_{T}}{I_{E_{1}}}$

From this, we find the following values: 
$R_{\pi_{1}} = 8.2k \Omega$
$R_{E_{1}} = 10k \Omega$
$I_{E_{1}} = 0.495mA \approx I_{c_{1}} \approx \beta \times I_{B_{1}}$

Now, we can use these values to find our resistances. 
$I_{1} = \frac{I_{E_{1}}}{\sqrt{ \beta }} = 38.2 \mu A$.
$R_{B_{1}} = \frac{V_{cc}- V_{C_{1}}}{I_{c_{1}}+I_{B_{2}}}$
$R_{E_{2}}= \frac{V_{E_{2}}}{I_{B_{2}}*\beta}$
$r_{\pi_{2}} = \frac{V_{T}}{I_{B_{2}}}$

Solving our system of equations, we get the following specifications for our resistors: 
$$R_{E_{2}} = 1k\Omega; \qquad R_{C_{1}} = 9.1k \Omega; \qquad R_{\pi_{2}} - 330 \Omega; \qquad I_{B_{2}} = 75.7 \mu A = \frac{I_{E_{2}}}{\beta}$$

Moving forward, we can choose our capacitance values such that we see a similar resistance of approximately $50 \Omega$, so we will have similar frequency. We specify a $C_{B}$ to be as small as possible as possible without changing the low cut frequency of 100Hz; adjusting the frequency by a factor of 10. 

$\omega_{3DBL} 1000 * 2\pi \approx \sqrt{ \left( \frac{1}{50 \Omega * C_{C_{1}}} \right)^{2} + \left( \frac{1}{50 \Omega *C_{C_{2}}} \right)^{2}} C_{C_{1}}= C_{C_{2}} = 4.7 \mu F; \qquad \omega_{C_{1}} = \omega_{C_{2}} = 4444 \frac{Rad}{s}$
$\omega_{CB} = \frac{1}{(R_{B_{1}}||R_{B_{2}}(r_{\pi} +R_{E_{1}}*(1+\beta))*C_{B}}=\frac{1}{90.83k \Omega \times C_{b}} < 4444 \frac{}{s}$
So $C_{B} = 0.027 \mu F$

Putting all of these values into our simulation, and running the DC operating point, we can find the equivalent input and output impedance. 

![[Pasted image 20251212163934.png]]

| Component   | Value          |
| :---------- | :------------- |
| $R_{E1}$    | $10k \Omega$   |
| $R_{C_{1}}$ | $9.1k \Omega$  |
| $R_{B_{1}}$ | $240k \Omega$  |
| $R_{B_{2}}$ | $160 k \Omega$ |
| $R_{E_{2}}$ | $160 k\Omega$  |
| $C_{C_{1}}$ | $4.7 \mu F$    |
| $C_B$       | $0.027 \mu F$  |
| $C_{C_{2}}$ | $4.70 \mu F$   |

Running the spice simulation at the DC operating point, we have that our circuit has the following solution: 

| Quantity | Value | Type |
|---------|-------|------|
| V(n003) | 0 | voltage |
| V(n005) | 8.11221 | voltage |
| V(n001) | 12 | voltage |
| V(output) | 7.4047 | voltage |
| V(n004) | 4.04428 | voltage |
| V(n002) | 4.67516 | voltage |
| V(n006) | 7.4047 | voltage |
| I(R2) | -2.92198e-05 | device_current |
| I(R3) | 3.05201e-05 | device_current |
| Ic(Q2) | 0.0073806 | device_current |
| Ib(Q2) | 2.4102e-05 | device_current |
| Ie(Q2) | -0.0074047 | device_current |
| Is(Q2) | 0 | device_current |
| I(R1) | -0.000404428 | device_current |
| I(C2) | -1.26229e-19 | device_current |
| I(C3) | 0 | device_current |
| Ic(Q1) | 0.000403128 | device_current |
| Ib(Q1) | 1.30037e-06 | device_current |
| Ie(Q1) | -0.000404428 | device_current |
| Is(Q1) | 0 | device_current |
| I(R4) | 0.00042723 | device_current |
| I(V1) | -0.00783835 | device_current |
| I(R5) | 0.0074047 | device_current |
| I(C1) | 1.90081e-17 | device_current |
| I(V2) | 1.90081e-17 | device_current |

Based on these values, we can determine $R_{0}$ and $R_{in}$ 

$$R_{o} = \frac{V_{Rms}}{I_{Rms}} = 46.7 \Omega; \qquad R_{in} = \frac{V_{Rms}}{I_{Rms}} = 54.20 \Omega$$

### Part B: Bode and Phase Plots in Spice
Now, to determine the midband gain of the circuit, we prove the output and find the midband. 

![[Pasted image 20251212163906.png]]


Midband gain: $A_{m} = \frac{V_{o}}{V_{in}} = \frac{103mV}{1mV} = 103 v / v$

This aligns with our simulated result: 
![[Pasted image 20251212162820.png]]

Also, measuring the $\omega_{L_{3DB}}$ and $\omega_{H_{3DB}}$. By Placing cursors 3dB below the midband region. We find that 

$\omega_{L_{3DB}} = 788 Hz$
$\omega_{L_{H3DB}}= 3.9MHz$

### Part C: Impedance for Voltage Source and Output
For this part of the question, I added in a equivalent series resistance of magnitude $50 \Omega$to the AC sweeping voltage source and a load resistor of magnitude $50 \Omega$. 
![[Pasted image 20251212164908.png]]

The resulting bode and phase plots of this simulation are: 
![[Pasted image 20251212165024.png]]

As we can see, low frequency is substantially above cutoff is substantially above 1kHz. To address this, we can adjust the capacitance of the base capacitor.

Small Aside: Even after troubleshooting and trying to make the base capacitor reflect the desired low frequency cutoff, it seemed as if my resistor values were far too large for the capacitance to make any real difference in the solution. So in consultation with my peers, I was able to come to this final result for the loaded circuit: 

![[Pasted image 20251212170706.png]]

This circuit had the following  Phase and Bode plot: 
![[Pasted image 20251212173546.png]]

**Discussion**
Using the 1/3 rule, we were able to solve for and specify the resistances and capacitances of the circuit that meet the impedance requirements. In the final part of the question I found that some of my values were over-specified which made lowering the lower 3dB value much more difficult. To address this, I changed many resistor values and my capacitor values. 

## Question 3: Second Order Active Low-Pass Butterworth filter. 

Below is an image of the circuit that we will be analyzing for this phase of the project. 
![[Pasted image 20251212174313.png]]

In particular, we have requirement that the sum of R1 and R2 is equal to 10,000 Ohms. 

The transfer function of this circuit is given by: 
$$H(s) = A_{m} \frac{\frac{1}{(RC)^2}}{s^{2}+s^\frac{(3-A_{m})}{RC}+\frac{1}{(RC)^2}}$$

In this transfer function, $A_{m}$ is the band pass gain $A_{m} = 1+ \frac{R_{1}}{R_{2}}.$
Since we are ultimately interested in the poles of the transfer function, we can can write the dampening constant as $\psi = \frac{3-A_m}{2}$, and the time constant $\frac{1}{RC}=\rho$

### Part A: Calculating Capacitance and Gain such that the Filter is a 2nd order low-pass Butterworth Filter and a 3dB cutoff of 10kHz. 

First, we know that for a cutoff frequency of $R=10k \Omega$, we can solve for C and obtain the following value. Cutoff frequency is given by $\omega_{C} = \frac{1}{RC}$. Since we know the cutoff frequency is 10kHz:
$$C=1.6nF$$
![[Pasted image 20251212175920.png]]
Above is an image describing the phase between the poles in a Butterworth filter. 


We want to find the values $A_{m}$ such that the circuit is a Butterworth filter. A second order Butterworth filter, has poles that are 45 degrees apart from the negative imaginary axis on the s-plane. The corresponding normalized Butterworth polynomials are $s^{2}+\sqrt{ 2 }s +1$. So $\sqrt{ 2 } = 3-A_{m}$. So, our mid-band gain is: 
$$A_{m} = 1.586$$
So then, Our corresponding Resistor values are: 
$$R_{1}=3.87k \Omega,\qquad R_{2} = 6.13k\Omega$$
After choosing all of our resistor values, and capacitor values, we can finally put the circuit into LTSpice. Luckily, the UA741 is a 50 year old op-amp, so all support for it is gone! I had to search the internet to find a model of it. 
![[Pasted image 20251212183428.png]]
![[Pasted image 20251212183522.png]]

As we can see from the magnitude and phase plot, the two poles will sit on the circle with a radius of the frequency $\omega_{c} = \frac{1}{RC} = 62500 \frac{rad}{s}$. The poles will be where the root locus contour starts at 45 degrees from the negative real axis. ![[root_locus.png]]

## Part B: Oscillation Gain. 

While ensuring that the sum of the resistors $R_{1}$ and $R_{2}$ is still $10k \Omega$, we increase $A_{m}$. At $A_{m} =3$, we notice the s term in the transfer function becoming 0. This is a crucial point in the transfer function, since this is when the system starts to oscillate. 

For the sake of simulation, I changed the capacitor values to $7k \Omega$ and $3k \Omega$. This showed me a very clear oscillations in the output, even when there was no input. 

Below is the circuit used to run the simulation: 
![[Pasted image 20251212190702.png]]

The simulation yielded the following output result, using the transient analysis method: 
![[Pasted image 20251212190726.png]]

From this output plot, we can see clear oscillations between the two rails of the op-amp. This oscillation frequency is measured to be $f_{osc} = 538kHz$

When the dampening constant from earlier is calculated, our values is 0 when our dampening constant is 0. $\psi = \frac{3-3}{2}=0$. This validates our result for oscillation in the filter. 

Finally, describing the locations of the poles in the s-plane can be done with a similar method as to what was done earlier for the non-oscillating filter. Since I increase the location of the poles for sake of visualizing, It makes sense that my poles are not exactly on the imaginary axis but a angled a bit more into the positive x. However, for exact values, we would just expect straight line rather than visible oscillations for a zero source-oscillator. 

![[root_locus_oscillating.png]]

Above is a plot of what the locations of the poles are when the dampening is equal to 0. 

**Discussion**
The final problem of this mini project gave me insight into how to properly design Butterworth filters such that we can choose the degree of smoothing in the frequency response, as well as the order of the filter. Lastly, the last part of the question was insightful into how to ensure our gain does not result in oscillating behavior of filters. 