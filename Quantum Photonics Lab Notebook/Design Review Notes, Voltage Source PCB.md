---
~
---

This document covers the general stages of the voltage source PCB design and gives justification for each part of its design. Thank you Adan and Mateo for your support throughout the project!

# Overview 

Our Voltage source can be used to supply four low noise voltage sources to cryo-amps / SNSPD for biasing with four right angled SMA connections. 

The voltage source PCB was designed with a few key ideas in mind. 

1. Low noise 
2. Absolute control 
3. Definitive knowledge about current-voltage relationship.

To ensure all those in the future are able to easily navigate the PCB and make further changes when needed, I will cycle back to these topics in every subsection. 
# PCB Layer Stack-up
This PCB was designed with a four layer stack-up in the following order. 

1. Signal 
2. Ground
3. Power 
4. Signal. 

This stack-up achieves a few key properties that make it optimal for our use case. These are discussed below: 
1. Signals on the top layer have minimal cross-talk due to a solid continuous ground plane beneath 
2. We achieve bulk capacitance between the PWR and GND layers, giving us less noise overall 
3. We have more space to route between bottom and top layers ensuring that traces can be spaced reasonably apart without making the board size too large. 

![[Pasted image 20260402134833.png]]
**Figure 1:** Layer Stack-Up

![[Pasted image 20260402134852.png]]
**Figure 2:** Additional Blind Vias to Support Layer Stack up

# Sections of the PCB 
This PCB is sectioned off into four distinct sections, this will assist with debugging in the future if (when) required. 

## Power Input and Regulation Stage

Our PCB connects directly to a wall outlet 120V/60Hz. There is sufficient bulk capacitance and small capacitance to deal with flicker noise. Furthermore, I included a transient voltage suppression (TVS) diode. 

The goal of this section is regulate our input voltage from a wall 9V DC connector to the following voltages: 
- +/- 5V
- +/- 2.5V
- 3.3V

This is primarily done through low dropout regulators (LDOs). This is done for a few reasons in particular noted below: 

1. Low output noise (expected in the $nv / \sqrt{Hz}$ to $\mu V \sqrt{Hz}$). 
2. Very low output current draw. This is ideal for our application dealing with both digital and analog components to reduce thermal noise. 
3. We also are able to facilitate higher signal speed with minimal interference from magnetic noise. 

For the negative voltage regulation, we first feed our 9V DC input through a charge pump inverter integrated circuit. Basically this takes our input and makes it *approximately* negative input. 
### Schematics: 
![[Pasted image 20260402140139.png]]
**Figure 3:** Positive voltage regulation

![[Pasted image 20260402140506.png]]
**Figure 4:** Negative voltage regulation

### Layout
![[Pasted image 20260402140553.png]]
**Figure 5:** Layout of voltage regulation stage 

A few notes on my layout for the voltage regulation stage. 
- Lefthand side is the positive voltage regulation. 
	- Vin -> 5V ->3V3 -> 2.5V
- Righthand side is negative voltage regulation
	- Vin ->-Vin -> -5V -> -2.5V
- Via stitching ground 
	- Gives us low impedance return paths to ground and reduces EMI.
## Micro-controller (ESP-32) Stage
The ESP-32 is the brains of our project. We will send commands over a serial bus (mini USB) to the ESP32 to control our output voltages. My personal goal for this was to make the ESP-32 as user-friendly as possible to all future employees / researchers that use it. For this reason I chose to go with a dev-kit board rather than a microprocessor. This allows for easy debugging of our signals when the time comes. 

Aside from this, a few considerations for the ESP-32 were that it had to have an interrupted ground plane to minimize EMI interference with other signals. This was easily achieved through my four-layer PCB design. 

The future plan for the ESP-32 board is to implement a gui with some sort of LLM, for now Adan has requested that we use the command line. If we run into issues with shipping etc. I will leave a base for the firmware for a future intern. 

### Schematic: 
![[Pasted image 20260402141615.png]]
**Figure 6:** ESP32 Schematic
### Layout: 
![[Pasted image 20260402141545.png]]
**Figure 7:** ESP-32 Layout
## Mixed Signal Stage 

The mixed signal stage of our Voltage Source PCB contains three main components. The Digital Analog Converter, the Differential Amplifier, and the Analog Digital Converter. 

### Digital Analog Converter (DAC)

For this project we decided to use the AD5761R. This specific DAC has incredible specifications for the purpose of this project. This choice of DAC also allows us to have multiple outputs over a serial peripheral interface (SPI) bus.

(SPI is a high speed digital communication protocol, in this project, all SOCs are controller with SPI)

**Relevant Specifications**
- 16 bit resolution $\implies 2^{16}$ adjustable steps 
- Low noise $35 nV / \sqrt{ Hz }$
- $7.5 \mu s$ setting time
- Bipolar configuration with $\pm 5V$

The DAC output is shunted through a 1K ohm resistor, which is then used for differential high-side current sensing. We also take note that this output is current limited by a 100, 000 ohm resistor later, meaning the peak current values are $-20 \mu A \text{to }+20\mu A$

Below are some schematic and layout screenshots
#### Schematic:
![[Pasted image 20260402143333.png]]
**Figure 8:** DAC schematic.
- Note the differential pair CS_P/N. We route this as a differential pair to ensure minimal EMI interference on our current sensing. 
- Decoupling input capacitors as well act as a low pass filter. 

#### Layout:
![[Pasted image 20260402143544.png]]
**Figure 9:** DAC layout
### Differential High-Side Current Sense Amplifier: 

The differential high-side current sense amplifier takes the voltage across the $1k \Omega$
resistor to determine the output current. We supply $\pm 2.5V$ to the amplifier and amplify our voltage by 125 times to output an analog signal from rail-to-trial. This signal is fed into an analog digital converter to be read by the ESP-32 at a later time. 

The amplifier I chose to move forward with, like all other components on this board has spectacular specifications when it comes to low noise outputs and handling EMI. For this differential amplifier we decided to use the ADA-4523. Below are some specifications: 

- Low noise $4.2 nv / \sqrt{ Hz }$
- Amplifier is unity-gain stable and provides rail to rail output swing (we can input voltage close to or equal to the rails while maintaining linearity).
- On chip filtering for high immunity to EMI. 
- Exceptional CMMR (160 dB) (basically only looks at the differential signals)
- Exceptional PSSR (168 dB) (power supply variations do not affect it)
- High precision ($4 \mu V$) offset voltage and low noise ensure small differential signals are not buried in amplifier noise or DC errors. 

#### Schematic:
![[Pasted image 20260402144830.png]]
**Figure 9:** DA Schematic
- Note low pass filtering at output will ensure we have minimal noise going into the ADC
#### Layout:
![[Pasted image 20260402144804.png]]
**Figure 9:** DA Layout
- trace length for differential signal is nearly identical ensuring no parasitic will impact it. 


### Analog to Digital Convert (ADC)

The analog to digital converter serves to take the analog signal from the current differential current sensing amplifier and send it back to the ESP32. For this task, we utilized a 4-input, multiplexed ADC with 24 bit resolution ($2^{24}$ steps of resolution). This fits our use case perfectly and reduces complexity on the board. For this task, we went with the AD7172-4. Below are the relevant specifications. 

- The device achieves 17.2 noise-free bits at the maximum output rate of 31.25 kSPS.
- It achieves a full 24 noise-free bits at an output rate of 5 SPS.
- The RMS noise can drop as low as 0.054 µV rms (at a 1.25 SPS output data rate using the Sinc3 filter with input buffers disabled)
- Consumes 1.5 mA of current

(note kSPS is kilo-samples per second)

The ADC feeds back into the ESP32 over the SPI bus and the user is able to read the outputs from any one of the DACs at a time. Below are schematics and layouts. 

#### Schematic
![[Pasted image 20260402145650.png]]
**Figure 10:** ADC Schematic
#### Layout
![[Pasted image 20260402145742.png]]
**Figure 11:** ADC Layout

# The Full Picture: 
I hope this document gives all readers a thorough insight to the design choices and thought process that went into this PCB. If there are any further questions regarding the design feel free to email me any time at avi.guha05@gmail.com. 

Below are some pictures of the full PCB 
![[Pasted image 20260402145843.png]]
**Figure 12:** Full Layout (all layers)

![[Pasted image 20260402150515.png]]
**Figure 13:** Full Layout (Top layer)
![[Pasted image 20260402150557.png]]
**Figure 14:** Full Layout (PWR layer)
![[Pasted image 20260402150540.png]]
**Figure 15:** Full Layout (GND layer)
![[Pasted image 20260402150620.png]]
**Figure 16:** Full Layout (Bottom layer)
![[Pasted image 20260402150714.png]]
**Figure 16:** Full Layout (Top & Bottom layer)
![[Pasted image 20260402150735.png]]
**Figure 18:** 3D View (Top)
![[Pasted image 20260402150802.png]]
**Figure 19:** 3D View (Bottom)

# Closing Remarks:

I'd like to once again thank Matteo and Adan for their support throughout the process. Especially the time accommodations with my classes this term. Hopefully this was an in depth (enough) review of the design choices and feel free to contact me with any further questions via email: avi.guha05@gmail.com.