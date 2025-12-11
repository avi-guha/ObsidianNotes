
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

From our initial requirements, we see that both values do satisfy our minimum input resistance of 3.5$k\Omega$. Furthermore, since they are quite close in value we can conclude that the calculations and simulations were done correctly. 
