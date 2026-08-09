# 13. JTAG, OpenOCD, and Hardware Debugging

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/12 Firmware Development|Previous: 12. Firmware Development]] | [[Guide Sections/14 Schematics and PCB Review|Next: 14. Schematics and PCB Review]]

## JTAG signals

- **TCK:** Test clock.
- **TMS:** Test-mode select.
- **TDI:** Test data input.
- **TDO:** Test data output.
- **TRST:** Optional test reset.

## JTAG capabilities

JTAG may allow you to:

- Read the device ID.
- Detect devices in a chain.
- Halt and resume the CPU.
- Single-step instructions.
- Set breakpoints.
- Set watchpoints.
- Read registers.
- Read and write memory.
- Program flash.
- Perform boundary scan.
- Access debug blocks before normal boot.

## OpenOCD

OpenOCD connects a debug adapter to a target through JTAG or SWD.

It commonly works with:

- GDB.
- J-Link.
- ST-Link.
- CMSIS-DAP probes.
- FTDI-based adapters.

## Debugging failed JTAG access

Check:

1. Target power.
2. Target voltage reference.
3. Ground connection.
4. Pin mapping.
5. JTAG versus SWD mode.
6. Reset state.
7. Debug clock speed.
8. Cable quality.
9. Boot configuration.
10. Security locking.
11. Device configuration file.
12. Chain order.
13. Signal integrity.

## Logic analyzer

Useful for digital signals such as:

- I2C.
- SPI.
- UART.
- GPIO.
- Reset.
- Interrupts.

A logic analyzer helps answer:

- Did communication occur?
- Was the address correct?
- Was an acknowledgement received?
- Was timing correct?
- Did the device stop responding?
- Was the packet formatted correctly?

## Oscilloscope

Useful for:

- Clock quality.
- Power-rail noise.
- Voltage droop.
- Reset timing.
- Signal rise and fall times.
- Analog behavior.
- High-speed signal quality.

## Protocol analyzer

Provides protocol-specific visibility for interfaces such as:

- PCIe.
- USB.
- Ethernet.
- MIPI.
- Other high-speed links.

## JTAG, SWD, and OpenOCD explained

- **JTAG:** Joint Test Action Group interface. It uses a clocked serial chain to access test and debug logic. In validation, JTAG can identify devices, halt CPUs, inspect registers, read/write memory, program flash, and perform boundary scan.
- **SWD:** Serial Wire Debug. It is an ARM debug interface that uses fewer pins than JTAG, typically SWDIO and SWCLK, while still supporting core halt, register access, memory access, and breakpoints on many ARM targets.
- **OpenOCD:** Open On-Chip Debugger. It is host software that talks to a debug probe on one side and a target chip on the other. It can expose a GDB server, program flash, issue reset/halt commands, and run target-specific scripts.
- **GDB:** GNU Debugger. With OpenOCD or a vendor server, GDB can set breakpoints, step code, inspect variables, and read memory on embedded targets.
- **J-Link, ST-Link, CMSIS-DAP, FTDI:** Common debug probes or adapter families. The probe converts USB commands from the host into JTAG or SWD electrical signaling for the target.
- **Boundary scan:** A JTAG feature that controls and observes chip pins through test cells. It can verify board connectivity without running normal firmware.
- **Debug clock speed:** The JTAG or SWD communication rate. A speed that is too high for the target, wiring, or board state can cause unreliable detection.

## How to interpret debug-access failures

If the probe cannot detect the target, first confirm physical prerequisites: target power, voltage reference, ground, connector orientation, pinout, reset state, and whether the target expects JTAG or SWD. If detection works but halt or memory access fails, the issue may be security lock, wrong target configuration, reset timing, inaccessible power domain, or a CPU that is held in reset.

JTAG success does not prove firmware is correct. It proves a debug path exists. Use it to determine where software execution stops, inspect reset vectors, read status registers, and recover evidence before resetting the board.

## Instrument selection

Use a logic analyzer when the signal is digital and protocol timing matters. Use an oscilloscope when voltage level, analog shape, noise, rise/fall time, clock quality, or power droop matters. Use a protocol analyzer when the link is too complex or too fast for simple digital decoding and you need packet-level interpretation.

---
