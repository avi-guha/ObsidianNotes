## Signal Modulation 
- variations in amplitude frequency or phase of a 'carrier' signal with the baseband signal. 
![[Pasted image 20260627154757.png]]

## Amplitude Modulation 
- mix together a carrier signal with the baseband signal. 
- ![[Pasted image 20260627155036.png]]
- $x_{am}(t) = A_{c} [1+mx_{bb}(t)]\cos(\omega_{c}t)$
	- m is the modulation index 

## Frequency Modulation vs Phase Modulation 
- Phase modulation: baseband is proportional to excess phase 
- Frequency modulation: baseband is proportional to excess frequencies 

## Radio Transceiver 
- two components:
	- transmitter
	- receiver 
	- ![[Pasted image 20260627155559.png]]

### Radio Transmitter 
- mix together carrier wave and baseband wave 
- after this amplify through power amplifier. 
- after power amplifier, pass through BPF to ensure nothing leaks into adjacent bands. 

#### specifications: 
- frequency bands and channels 
- types of modulation 
- data rates 
- tx output power
- tx spectra mask  

### Radio Receiver 
- filter out out of band noise 
- pass through LNA 
- Mix with local oscillator (why?) 
	- DPD maybe?
- LPF after 
- IF output?
![[Pasted image 20260627160740.png]]

#### Wireless standard specs 
- RX sensitivity 
- RX Dynamic rnage 