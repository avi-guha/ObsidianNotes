# 14. Schematics and PCB Review

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/13 JTAG, OpenOCD, and Hardware Debugging|Previous: 13. JTAG, OpenOCD, and Hardware Debugging]] | [[Guide Sections/15 Automated Validation Infrastructure|Next: 15. Automated Validation Infrastructure]]

## Major schematic blocks

Be able to identify:

- Input power.
- Regulators.
- PMIC.
- Decoupling.
- SoC.
- Memory.
- Clock sources.
- Reset circuits.
- Boot straps.
- Debug connectors.
- Communication buses.
- High-speed interfaces.
- Level shifters.
- Protection components.
- Sensors.
- Test points.

## Power review questions

- Are all required rails present?
- Are voltage values correct?
- Is sequencing correct?
- Are enable pins controlled?
- Are power-good signals used correctly?
- Is decoupling adequate?
- Can each rail be measured?
- Is the regulator rated for peak current?
- Are sense lines routed correctly?
- Are analog and digital rails treated appropriately?

## Reset review questions

- What asserts reset?
- What releases reset?
- Is reset held long enough?
- What happens during brownout?
- Are pull-ups or pull-downs correct?
- Does every dependent device reset correctly?

## Clock review questions

- Is the frequency correct?
- Is the voltage standard correct?
- Is termination required?
- Is the source enabled at startup?
- Is jitter acceptable?
- Can the clock be probed?
- Are differential polarity and routing correct?

## Digital bus review

### I2C

- Correct pull-up voltage.
- Appropriate pull-up resistance.
- Unique addresses.
- Voltage compatibility.
- Bus capacitance.
- Accessible test points.

### SPI

- Correct chip-select behavior.
- Correct clock polarity and phase.
- Proper voltage levels.
- Signal integrity.
- Pull resistors where required.

### UART

- Correct TX/RX crossing.
- Correct voltage standard.
- Ground reference.
- Accessible connector.

## High-speed review

- Controlled differential impedance.
- Length matching.
- Minimal stubs.
- Appropriate termination.
- Clean reference plane.
- Return-current continuity.
- Connector suitability.
- Via count.
- Lane polarity.
- AC-coupling capacitor placement.
- Isolation from noise sources.

## Debug-access review

A validation-friendly board should expose:

- JTAG or SWD.
- UART.
- Major test points.
- Reset control.
- Boot straps.
- Rail measurement points.
- Clock test points.
- Independent power control where possible.

## Schematic terms in more detail

- **PMIC:** Power Management IC. It may generate several rails, control sequencing, monitor faults, and expose status through I2C or GPIO.
- **Regulator:** A circuit or IC that converts one voltage to another. Buck regulators step down efficiently; LDOs are simpler and quieter but less efficient for large voltage drops.
- **Decoupling capacitor:** A local charge reservoir near an IC power pin. It helps supply fast transient current and reduces rail noise.
- **Pull-up or pull-down:** A resistor that defines a default logic level when no active driver is present. Boot straps, reset lines, I2C, and mode pins often depend on correct pull values.
- **Level shifter:** A circuit that translates signals between voltage domains, such as 1.8 V logic and 3.3 V logic.
- **Test point:** A physical pad or loop that allows probing a signal or rail during bring-up and failure analysis.
- **Controlled impedance:** PCB trace geometry designed to match a target impedance for high-speed signals.
- **Return path:** The path current takes back to its source. Broken return paths create noise, emissions, and signal-integrity problems.
- **AC-coupling capacitor:** A series capacitor used on many high-speed differential links to block DC offset while passing transitions.

## Review mindset

During schematic review, ask how you would debug the board if it failed. A validation-friendly design exposes rails, reset, clocks, boot straps, debug ports, UART, and important error signals. A design may be functionally correct but painful to bring up if critical signals cannot be measured or controlled.

For high-speed signals, schematic review and layout review must connect. The schematic may show the right components, but the layout determines impedance, length matching, stubs, via count, reference planes, and crosstalk.

---
