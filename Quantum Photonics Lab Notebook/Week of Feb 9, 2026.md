
# Feb 9
- finished up the design for the cage that fits directly on Adan's probe. 
	- Includes screws for security
	- Fits well with the smaller PCB (2 SMP connectors).

# Feb 10
- Design phase of the low noise tunable voltage source. 
- Ideas: 
	- ADC with negative pin. 
	- Need a negative voltage source (charge pump inverter)
	- Multiple LDO's for precision voltage. 
	- output will be microcontroller dependent. 
	- Relevant parts: 
		- AD5791 - DAC
		- LT3094 - negative LDO 
		- LT 3042 - positive LDO
		- Need 5V line and -5V line. 
		- We use software to specify our limits. (-1.5V to 1.5V)
		- ICL7660 for charge pump to -5V

# Feb 11 
- From LDO datasheet, value for Rset
- ![[Pasted image 20260211150756.png]]
- Ran into some shitty ripple output from the LDO - realized this was because with the added load, our charge pump no longer supplies the required voltage to the LDO. 
- Fix: change the input to 5.5V using the MT3608 boost converter. 

# Feb 12 
- ![[Pasted image 20260212143356.png]]
- Charge pump circuit 
- ![[Pasted image 20260212143420.png]]
- Negative LDO
- ![[Pasted image 20260212144454.png]]
- positive LDO
- Switching to AD5761R DAC for good blessings (not ridiculous bullshit voltage requirements)
- ![[Pasted image 20260212175516.png]]
- ![[Pasted image 20260212175537.png]]
- 