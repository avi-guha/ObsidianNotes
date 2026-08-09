---
~
---

### Ericsson 
- Designed and reviewed high speed tranceiver schematics // signal integrity, gain and bw
	- Currently the platform is done 
	- I have worked on frequency band variation 
	- Simulating new matching networks and filters to minimize oobe 
		- Using cadence AWR
	- ensuring linear gain of LNA and TIA. 
	- **Transmitter Chain**
		- Baseband & DAC 
			- Convert raw digital to analog by I/Q signals 
		- Modulator / Mixer 
			- Combines I/Q with local oscillator to upconvert to desired RF 
		- Driver Amplifier 
			- Preiliminary gain to prep signal for final amplification 
		- BPF 
			- get rid of OOB frequencies 
		- Power Amplifier 
			- Boost signal strength significantly for long distance transmitting 
	- **Receiver**
		- LNA 
			- Amplifies the weak signal 
		- RF Filter 
			- get rid of OOBE 
		- Demodulator 
			- Down-convert to IF or Baseband I/Q components 
		- ADC 
			- Samples processed I/Q to convert to digital front end
- Designed multi-layer PCBs for mixed signal and RF systems using allegro. 
	- Power delivery networks 
	- I2C and SPI routing 
	- Some cool shapes that have specific properties (large area traces for inductance)
	- reviewing design constraints 
- AI agent to automate elec component derating 
	- internal API calls 
	- grabbing BOM and application sheets 
	- automatically comparing components and providing suggestions 
- Interesting notes 
	- only through hole vias are used for price purposes 
	- all boards are powered with -48V for dust repellence 
	- capacitors 

### Thunderbots 
- Led design of power distribution board 
	- flyback converter 
	- galvanic isolation 
	- feedback 
- SPI - CAN interface board 
	- CAN differential 
	- mitigate effects of EMI on SPI lines 
- TIA (X) DA ($\checkmark$) 
	- filtering out frequency accurately 
	- speccing amplifier to handle high frequency noise with CMMR 
- Motor control 
	- Firmware + S-curve motion profiles lead to efficient motor control 
	- PID tuning for controlling the robot position. 
### QSiP researcher 
- SNSPD
	- basically quantum mechanics electrical system for photon detection 
- voltage source board 
	- precise current sensing and voltage sensing 
- RF Amplifier 
	- LNA maximizes signal to noise ratio from photon detectors 
- Built ESP-32 firmware 
	- just C for ADC and DAC control. 

### 2D Physics Research 
- MXenes for MEMS / NEMS 
	- carbide, nitride, carbonitride 
- PID controlled fluid cell, humidity effect on 2-D materials 
	- bubbler 
	- servo valves 
- TEM sample holder
	- UHV design tolerances 
	- solidworks 

Zipline 
10 years old africa 
rural env 
urban platform 


**Interview Style**
- Round 1: 
	- recruiter screen, describing role 
- Round 2: 
	- engineering manager, resume review 
	- mostly personal 
- Round 3: 
	- Direct team, two parts 
		- part 1. behavioural - skipped 
		- part 2. technical 
			- resume review 
				- describing projects etc 
			- Technical questions 
				- antenna design 
				- RF design 
				- Describing the system 
					- TBots 
				- Fourier Transforms 
					- SINC function 
					- a sine wave 
			- General interest in the projects they are working on. 