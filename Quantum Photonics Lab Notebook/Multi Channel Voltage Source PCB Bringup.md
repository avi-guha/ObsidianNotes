**Goal:** This document provides instructions for bringing up the multi-channel voltage source PCB.

# Overview

This bringup should be done in stages. Do not connect a cryo-amp, SNSPD, or other sensitive load until the power rails, firmware interface, DAC outputs, current sense path, and output noise have been checked.

Recommended equipment:
- Bench supply with current limit
- Digital multimeter
- Oscilloscope
- Logic analyzer or oscilloscope SPI decode
- Known resistive loads
- Spectrum analyzer or low-noise baseband analyzer
- Low-noise amplifier (LNA), DC block, and shielded coax

## Pre-Power Inspection

- Visually inspect the PCB before applying power.
	- Check component orientation for polarized capacitors, regulators, charge pump, DACs, ADC, current sense amplifiers, ESP32 module, and SMA connectors.
	- Check for solder bridges on fine-pitch packages and connector pins.
	- Confirm all DNI / DNP parts match the schematic.
- Confirm the input supply polarity before connecting the board.
- Set the bench supply current limit low for first power-up.
	- Start around 50 mA to 100 mA if the expected idle current is unknown.
	- Increase only after the input rail and local regulators are confirmed.

## Smoke Test

- The goal of the smoke test is to catch shorts, incorrect assembly, and rail faults before the active circuits are exercised.
- Keep all SMA outputs unloaded during the first power checks.
- Do not plug in the ESP32 USB and external 9 V input at the same time unless the schematic explicitly supports that power path.

### Shorts to Ground

- With the board unpowered, measure resistance from each rail to ground.
	- Vin
	- +5 V
	- -5 V
	- +2.5 V
	- -2.5 V
	- 3V3
- Compare the readings against the schematic and assembled loads on each rail.
- Investigate any rail that measures as a near-short before applying power.
- Also check resistance between adjacent rails where accidental solder bridges are plausible.

### First Power-Up

- Power the board from a current-limited 9 V supply.
- Bring the current limit up slowly while watching the supply current.
- If the board immediately current-limits, power down and re-check for shorts or incorrectly installed parts.
- Confirm Vin at the board input after the protection and input filtering stage.
- Let the board sit powered for a short period and check for heating on regulators, charge pump, DACs, ADC, current sense amplifiers, and the ESP32 module.

### Voltage Rail Check

- Probe the outputs of the LDOs and charge inverter.
	- +5 V
	- -5 V
	- +2.5 V
	- -2.5 V
	- 3V3
- Confirm each rail is within the expected regulator tolerance.
- Check rail ripple/noise on the oscilloscope with short ground spring probing.
- Measure the voltage at the IC supply pins, not only at the regulator outputs.
- Check continuity from each regulator output to its intended loads.
	- Expected trace resistance should be very low; investigate any unexpected open or high-resistance path.
- Power-cycle the board after the rail checks and confirm the rails come up repeatably.

## Firmware Check

- Confirm the ESP32 enumerates over USB and can receive firmware.
- Flash a minimal firmware image that can control the SPI bus and print status over serial.
- Verify the basic digital pin states before talking to the analog ICs.
	- Chip-select lines should idle high.
	- SCLK should be idle in the SPI mode required by the DAC and ADC.
	- MOSI should not chatter unexpectedly when no transaction is active.
- Send test SPI transactions and verify with a logic analyzer or oscilloscope.
	- Confirm the intended chip-select line goes low for each target device.
	- Confirm no other chip-select line glitches during the transaction.
	- Confirm the SPI word length, bit order, clock polarity, and clock phase match the datasheets.
- If the firmware supports per-channel commands, step through all four channels and confirm the selected channel matches the expected DAC output path.

## DAC Output Check

- Start with all DAC outputs commanded to 0 V or midscale, depending on the configured DAC range.
- Measure each SMA output with no load connected.
- Step each DAC through a small set of known codes.
	- Negative full-scale or a safe negative test code
	- 0 V / midscale
	- Positive full-scale or a safe positive test code
- Confirm the measured output voltage follows the expected transfer function for the configured DAC range.
- Repeat the check with a known resistive load.
	- Verify the output resistor limits current as expected.
	- Confirm the output voltage does not collapse unexpectedly under the test load.
- Check for channel-to-channel coupling by holding three channels fixed while stepping the fourth channel.

## Current Sense and ADC Check

- Apply known output voltages into known resistive loads so the output current can be calculated.
- Measure the voltage across the current sense resistor directly with a DMM or oscilloscope.
- Confirm the current sense amplifier output moves in the correct direction and stays within its valid output range.
- Read the ADC value through firmware and compare it against the expected current.
- Repeat for each output channel.
- Record the offset reading with zero commanded output current so it can be subtracted in firmware or calibration notes.

## Noise Figure Test

- The goal of this test is to estimate the board output noise without letting the measurement instrument dominate the result.
- This is really an output noise-density measurement; the LNA is used only to lift the board noise above the analyzer noise floor.

### Setup

- Use the following signal chain:

$$\text{Voltage source output} \rightarrow \text{DC block} \rightarrow \text{LNA} \rightarrow \text{spectrum analyzer}$$

- Use a DC block before the LNA.
	- The board output can sit at a DC bias voltage.
	- Feeding DC directly into the LNA can saturate or damage it.
- Use short shielded coax between the board, DC block, LNA, and analyzer.
- Power the LNA from batteries or a known low-noise linear supply.
- Place the board and LNA in a grounded shielded enclosure if 60 Hz pickup or RF pickup dominates the measurement.

### Baseline Measurements

- Measure the analyzer noise floor with its input terminated.
- Measure the LNA + analyzer noise floor with the LNA input terminated in the impedance required by the LNA.
- Record the LNA gain, input-referred noise density, analyzer resolution bandwidth, and measurement bandwidth.
- Confirm the LNA output does not compress or clip during the baseline measurement.

### Board Measurement

- Command the DAC channel under test to the desired DC output voltage.
- Confirm the DC voltage with a DMM before connecting the LNA chain.
- Connect the output through the DC block and LNA to the analyzer.
- Measure the output noise density over the bandwidth of interest.
- Repeat for each output channel and for any DAC codes that matter for the intended experiment.
- Watch for narrowband spurs separately from broadband noise.
	- 60 Hz and harmonics usually indicate grounding or shielding pickup.
	- Switching-frequency spurs usually indicate supply or charge-pump coupling.
	- SPI-correlated spurs usually indicate digital activity coupling into the analog output.

### De-Embedding

- Treat the board noise, LNA noise, and analyzer noise as uncorrelated noise sources.
- Use voltage noise density units consistently.

$$e_{n,\text{meas}}^2 = (e_{n,\text{board}}^2 + e_{n,\text{LNA}}^2)A_v^2 + e_{n,\text{inst}}^2$$

$$e_{n,\text{board}} = \sqrt{\frac{e_{n,\text{meas}}^2 - e_{n,\text{inst}}^2}{A_v^2} - e_{n,\text{LNA}}^2}$$

Where:
- $e_{n,\text{meas}}$ is the measured analyzer input noise density.
- $e_{n,\text{board}}$ is the board output noise density.
- $e_{n,\text{LNA}}$ is the LNA input-referred noise density.
- $A_v$ is the LNA linear voltage gain.
- $e_{n,\text{inst}}$ is the analyzer input-referred noise density.

- If the LNA gain is high enough that the analyzer noise is negligible, the $e_{n,\text{inst}}$ term can be dropped.
- Log the final result as noise density versus frequency for each channel, not only as a single integrated RMS value.

## Bringup Record
- Record the board serial number or assembly identifier.
- Record the test date, operator, firmware revision, and power supply settings.
- Save the measured rail voltages, idle current, DAC transfer measurements, ADC/current sense measurements, and noise spectra.
- List any rework performed during bringup.

## Next steps: 
- Adjusting schematics based on debugging. 
- 