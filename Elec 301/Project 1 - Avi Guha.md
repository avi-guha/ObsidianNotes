# Problem 1

![[Pasted image 20251116152831.png]]

We can use the method of open circuit and short circuit time constants to find the low-frequency and the high-frequency 3-Db points and the transfer function. 

### Midband Gain:

$$A_{m} = \frac{V_{0}}{V_{i}} = \left( \frac{R_{2} || (R_{3}+R_{4})}{R_{1}+R_{2}||(R_{3}+R_{4})} \right)\left(\frac{R_{4}}{R_{3}+R_{4}}\right)= 0.4348$$

### Zeros:
There is a contribution of zeros by $C_{1}$ and $C_{3}$ at $\omega = 0$ Hz, this is due to the transfer function accounting for high pass capacitors with a 's' in the numerator. 

### Dominant Poles: 

At low frequency, C2 and C4 have no effect, so we examine the circuit without their presence, then we use the short circuit method on each remaining capacitor, C1 and C3 to determine the time constant and then the dominant pole. 

$\omega_{C_{1}} = \frac{1}{(R_{1} + R_{2}||(R_{3}+R_{4}))C_{1}} = 130.435 \; rad / s$

$\omega_{C_{3}} = \frac{1}{((R_{1} || R_{2}) +R_{3} +R_{4})C_{3}} = 1913.043 \; rad / s$

$\omega_{L3db} = \sum \omega = 2043.478 \; rad / s$

$f_{L3db} = \frac{\omega }{2 \pi} = 325.2 \; Hz$


At lower frequencies, the dominant pole is the pole that is with highest frequencies. So $C_{3}$ is the dominant pole. Now we can image the $C_{1}$ has no idea about $C_{3}$ and treat as an open circuit. So we recalculate $C_{1}$

$\omega_{C_{1}} = \frac{1}{(R_{1}+R_{2})C_{1}}=91 \; rad / s$

High Frequency 3DB point is determined by the low pass capacitors, $C_{2}$ and $C_{4}$. We use the open circuit method on each method to determine the time constant and the dominant pole. 

$\omega_{C_{2}} = \frac{1}{(R_{1}||R_{2}||(R_{3}+R_{4}))C_{2}} = 2.300 \cdot 10^{8} \; rad / s$
$\omega_{C_{4}} = \frac{1}{(((R_{1}||R_{2})+R_{3})||R_{4})C_{4}} = 3.833 \cdot 10 ^{7}\; rad / s$

$\omega_{H_{3db}} = \left( \sum \frac{1}{\omega} \right)^{-1} = 3.29 \cdot 10 ^{7}\; rad /s$
$f_{3dB} = \frac{\omega}{2\pi} = 5.23 \cdot 10^{6} \; Hz$

At higher frequencies, the dominant pole is that which acts at a lower frequency. We see from the calculations that $C_{4}$ is the dominant pole, then we can treat this as a short circuit.

$\omega_{C_{2}} = \frac{1}{(R_{1}||R_{2}||R_{3})C_{2}} = 2.4 \cdot 10^{8} \; rad / s$ 

Now that we know the poles, and zeros, we can determine the transfer function easily. 

$T(s) = A_{m} F_{L}(s) F_{H}(s)$
$T(s) = 0.4348 \left(  \frac{s^{2}}{(s+90.909)\left( s+1913.043 \right)} \right) \left(  (\frac{1}{ (1 + \frac{s}{3.833 \cdot 10^7})(1+\frac{s}{2.4 \cdot 10^8})} \right)$

## 1 A: 
LT Spice Schematic: 
![[Pasted image 20251116164836.png]]

Corresponding BODE Plot
![[Pasted image 20251116164908.png]]

We can estimate the poles and zeros of the circuit by inspecting the slope at certain sections. 


The poles of this Bode Plot occur at approximately 15 Hz, 300 Hz, 5 MHz and 42 MHz. Also from the phase plot we can see there are two zeros at 0 Hz. 
## 1 B:
To simulate all five at the same time to accurately compare the effect of C3, I copied and pasted four more of the circuit. As shown below: 
![[Pasted image 20251116170403.png]]
This is the corresponding bode plots. 
![[Pasted image 20251116170324.png]]

To determine the corresponding $F_{3db}$ point, simply draw at 3dB to then measure the corresponding frequencies. 

The results were as follows: 

| C     | F3dB  |
| :---- | :---- |
| 500nF | 310Hz |
| 1uF   | 160Hz |
| 2uF   | 87Hz  |
| 5uF   | 40Hz  |
| 10uF  | 30Hz  |

Now, using calculations to find the $f_{3dB}$ point, we can use the following formula: 

$$f_{L_{3dB}} = \frac{1}{2\pi}\left( \omega_{c_{1}}+\frac{1}{((R_{1}||R_{2} +R_{3} +R_{4}))C_{3}} \right)$$
Which yields us the following results: 

| C3    | Simulated (Hz) | Calculated (Hz) | Error |
| :---- | :------------- | :-------------- | :---- |
| 500nF | 310            | 325             | 4%    |
| 1 uF  | 160            | 172             | 7%    |
| 2 uF  | 87             | 97              | 12%   |
| 5 uF  | 40             | 51              | 19%   |
| 10 uF | 30             | 36              | 20%   |
Percent error was calculated with the following formula: 

$$\text{Error} = \frac{|\text{calculated - simulated}|}{\text{simulated}} \cdot 100\%$$
Notes: 
- Increasing the capacitance, has a inverse effect with the low frequency 3dB cutoff. Furthermore, we also notice an increase in the error as a trend. 
# Problem 2

## 2A
![[Pasted image 20251116173556.png]]

This is the LT-Spice Schematic for Problem 2. Sorry about it being so ugly with the current source. 
![[Pasted image 20251116173404.png]]

To use Miller's theorem, we can say that C1 and C4 are high-pass capacitors, and C2 is low pass capacitor. Then we recognize that C3 is a floating capacitor between the two signal nodes that have a gain between them. 
- Key points are: 
	- Left of C3 is the node that drives the transconductance stage 
	- Right of C3 is the output of the stage. 
	- Two nodes do not sit at the same AC voltage, so there is a finite gain between the two. 

Out first step is to find the voltage gain between $V_{1}$ and $V_{0}$. We assume that C1 and C4 are short circuits, and C2 is an open circuit. We can use this information to find K, the voltage gain from $V_{1}$ to $V_{0}$
$$V_{0}= KV_{1}$$
$$K = -G(R_{3}||R_{4}) = -100$$
Now that we have K, we can use this information to find $C_{3L}$ and $C_{3R}$, which are the equivalent capacitances of C3 on the left and right hand sides of the circuit. 

$$C_{3L} = C_{3}(1-K) = 202\; pF$$
$$C_{3R} = C_{3}\left( 1-\frac{1}{K} \right) = 2.02\; pF$$
So, after finding these values, we get the following transformed circuit: 
![[Pasted image 20251116180143.png]]

$$A_{m} = \frac{V_{o}}{V_{s}} = \frac{V_{1}}{V_{s}} \cdot \frac{V_{o}}{V_{1}}$$
Is the formula for our midband gain. Continuing, we get: 
$$A_{m} = \left( \frac{R_{2}}{R_{1}+R_{2}} \right)(K) = -95.24$$
Determining the zeros, we see that $C_{5}$ and $C_{8}$ are both zeros at $\omega = 0$, by inspection.

Now, to calculate the 3dB point and the low frequency poles using the short circuit time constant method. We remove the low pass capacitors, since we are examining low frequencies and how the high-pass capacitors will behave. 

$\omega_{C_{1}} = \frac{1}{R_{1}+R_{2}}C_{1} = 238 \; rad / s$
$f_{1} = 38Hz$

$\omega_{C_{4}} = \frac{1}{R_{3}+R_{4}}C_{4} = 125 \; rad / s$
$f_{2} = 20 Hz$

So the L3dB pole is at the sum of the two low pole frequencies. 
$f_{L_{3dB}} = \sum f = 58 \; Hz$

Next, to calculate the 3dB point at the high frequency pole, we use the open circuit time constant method. Here, we want to examine the low-pass capacitors,  so we ignore the high-pass capacitors, and use the open circuit time constant method. 

$\omega_{C_{2} + C_{3L}} \frac{1}{(R_{1}||R_{2})(C_{2}+C_{3L})} = 1 \cdot 10^{8} \; rad / s$
$f_{1} = 1.6 \cdot 10^{7} \; rad / s$

$\omega_{3R} = \frac{1}{(R_{3}||R_{4})C_{3R}} = 5 \cdot 10^{8} \; rad / s$
$f_{2} = 7.8 \cdot 10^{7} Hz$

$f_{H_{3dB}} =\left(  \sum \frac{1}{f} \right) ^{-1} = 13.8 \; MHz$

## 2B
![[Pasted image 20251116182656.png]]
Knowing there are two zeros at 0Hz and a zero at 8Ghz, by inspection of where the slope starts to increase, we can make estimates of the location of our poles. So we find that our poles are at approximately 15Hz, 55Hz, 15MHz, 2GHz. 

We can compare the poles and zeros on the Bode Plot and the calculated values: 

|            | $f_{z_{1}}$ | $f_{z_{2}}$ | $f_{z_{3}}$   | $f_{p_{1}}$ | $f_{p_{2}}$ | $f_{p_{3}}$ | $f_{p_{4}}$ |
| :--------- | :---------- | :---------- | :------------ | :---------- | :---------- | :---------- | ----------- |
| Simulated  | 0Hz         | 0Hz         | 8Hz           | 15Hz        | 55Hz        | 15MHz       | 2GHz        |
| Calculated | 0Hz         | 0Hz         | $\textemdash$ | 20Hz        | 38Hz        | 16MHz       | 78MHz       |
We can estimate the low-frequency and high-frequency 3-dB points by drawing a 3dB below the midband gain, and estimating where it intersects the Bode Plot. The estimated low frequency 3dB point is at 50Hz and the estimated high-frequency 3-dB point is at 15MHz. 

|            | $f_{L_{3dB}}$ | $f_{H_{3dB}}$ |
| :--------- | :------------ | :------------ |
| Calculated | 57.79 Hz      | 13.8 MHz      |
| Simulated  | 50 Hz         | 15 MHz        |
| Error      | 15.6%         | 8.9%          |
# P3.
![[Pasted image 20251116192153.png]]

## 3A: 

In order to get the s.s. BJT model, we want to first find the DC operating point. For further simplifications we will assume the base emitter voltage is a constant 0.7V, we will also neglect the early effect. 

We simplify the base circuit to a Thevenin equivalent circuit. With the Thevenin equivalent circuit for the base, we can solve for base current, $I_{b}$ using Kirchoff's Voltage Law, to later get the collector current, $I_{c}$.

$$V_{th} = V_{bat} \frac{R_{b}}{3R_{b} + R_{B}} = \frac{1}{4} V_{BAT}$$
$$R_{TH} = 3R_{B}||R_{B}$$
$$V_{Th} - I_{B}(3R_{B}||R_{B}) -V_{BE} - I_{B}(\beta + 1)(R_{E}) = 0$$
$$I_{B} = \frac{V_{Th} - V_{Be}}{(3R_{B}||R_{B}) + (\beta + 1)(R_{E})} = 7.91 \mu A$$
$$I_{c} = \beta I_{B} = 1.58 mA$$

Since we have solved for $I_{B}$, we can now find the small-signal values $r_{\pi}, g_{m},$ and $r_{o}$

Assuming that $V_{T} = 25mV$, we have that: 

$r_{\pi} = \frac{V_{t}}{I_{B}} = 3159 \Omega$
$g_{m} = \frac{I_{c}}{V_{T}} = 0.0633 S$
$r_{o} = \frac{V_{A}}{I_{C}} = 126363 \Omega$

With these values in mind, we can create the small-signal model and then solve for $R_{i,t}, R_{i}, R_{o,t}$, and $R_{o}$. In the midband $C_{\inf}$ capacitors become short circuits, while the $C_{\pi}$ and $C_{\mu}$ capacitors are open circuits. Based on this, our spice model becomes: 
![[Pasted image 20251116195328.png]]

Solving for  $R_{i,t}, R_{i}, R_{o,t}$, we have: 
$$R_{i,t} = r_{\pi} = 3159 \Omega$$
$$R_{i} = 3R_{B} || R_{B} || R_{i,t} = 2610 \Omega$$
$$R_{o,t} = r_{o} = 126364 \Omega$$
$$R_{o} = R_{c} || R_{o,t} = 3877 \Omega$$
We can also verify this in simulation by placing a 1A current source around various parts of the circuit, then the potential difference across the current source will then be our resistance in ohms. 
![[Pasted image 20251116222702.png]]


## 3B: 
In the next stage, we remove $C_{e} = C_{\inf}$ from the circuit. This means that $R_{E}$ is no longer shunted. 

Our LTSpice Model becomes the following (finally found out how to mirror): 
![[Pasted image 20251116213602.png]]
Below is our solution to the input and output resistances: 

$R_{i,t} = r_{\pi} + \frac{R_{E}r_{o}(\beta + 1) + R_{E}(R_{C} || R_{L})}{R_{E} + r_{o} + (R_{c} || R_{L})} = 38.8 \cdot 10^{5} \Omega$
$R_{o,t} = \frac{(r_{s} || 3R_{B} || R_{B})(R_{E} + r_{o}) + (\beta + 1)R_{E}r_{o}+R_{e}r_{\pi}+r_{o}r_{\pi}}{(r_{s}||3R_{B} ||R_{B}) + R_{E} + R_{\pi}} = 9.739876 M \Omega$
$R_{o} = R_{c} || R_{o,t} = 3998 \Omega$

We can also verify this in simulation by placing a 1A current source around various parts of the circuit, then the potential difference across the current source will then be our resistance in ohms. 
![[Pasted image 20251116223029.png]]
Comparing the two values in a table: 

|     | $R_{i}$        | $R_{i,t}$       | $R_{o}$       | $R_{o,t}$        |
| --- | -------------- | --------------- | ------------- | ---------------- |
| 3A  | 2610 $\Omega$  | 3159 $\Omega$   | 3877 $\Omega$ | 126364 $\Omega$  |
| 3B  | 14442 $\Omega$ | 388049 $\Omega$ | 3998 $\Omega$ | 9739876 $\Omega$ |
So as we can see from a general trend, the presence of the emitter resistor increases the input and output resistances. This contrasts what has typically been learned in class, however this is due to the the class typically assuming $r_{o} =  \infty$

# Part 4

Since the values in P4 are the same as that in P3, we can reuse values $r_{\pi}, g_{m}$ and $r_{o}$ values calculated earlier, for our s.s. BJT model in midband. 

## 4A:
Below is the Spice model for 4A: 
![[Pasted image 20251116212704.png]]
After calculating our input and output resistances, we find: 
$R_{i,t} = (R_{c} || R_{L})r_{\pi} + \frac{r_{o}r_{\pi}}{(\beta +1)r_{o} + (R_{c}||R_{L})} = 16.2 \Omega$
$R_{i} = R_{E} || R_{i,t} = 16\Omega$

$R_{o,t} = \frac{(\beta + 1)(R_{E} || r_{s})r_{o} + (R_{E}||r_{s})r_{\pi} + r_{o}r_{\pi}}{(R_{E}||r_{s}) + r_{\pi}} = 866064 \Omega$
$R_{o} = R_{C}||R_{o,t} = 3982 \Omega$

We can also verify this in simulation by placing a 1A current source around various parts of the circuit, then the potential difference across the current source will then be our resistance in ohms. 



![[Pasted image 20251116221942.png]]
Comparing the case in problem 3A, to that of problem 4A: 

|     | $R_{i}$        | $R_{i,t}$      | $R_{o}$       | $R_{o,t}$       |
| :-- | :------------- | :------------- | :------------ | :-------------- |
| 3A  | 2610 $\Omega$  | 3159 $\Omega$  | 3877 $\Omega$ | 126364 $\Omega$ |
| 4A  | 16.04 $\Omega$ | 16.16 $\Omega$ | 3982 $\Omega$ | 866064 $\Omega$ |
As we can see, the input resistances for $R_{i}$ and $R_{i,t}$ for 4A are substantially smaller than what we found in 3A. However, the output resistance is substantially larger in 4A than in 3A. It should be noted that they are still relatively similar in terms of order of magnitude.


