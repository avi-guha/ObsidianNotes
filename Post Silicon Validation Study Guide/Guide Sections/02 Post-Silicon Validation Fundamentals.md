# 2. Post-Silicon Validation Fundamentals

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/01 Understanding the Role|Previous: 1. Understanding the Role]] | [[Guide Sections/03 Validation Planning and Test Design|Next: 3. Validation Planning and Test Design]]

## What is post-silicon validation?

Post-silicon validation tests a real physical chip after it has been fabricated.

The silicon is placed on a board and tested using:

- Embedded firmware.
- Operating-system software.
- Synthetic workloads.
- JTAG or SWD.
- Oscilloscopes.
- Logic analyzers.
- Protocol analyzers.
- Bit-error-rate testers.
- Power supplies.
- Thermal chambers.
- Automated test scripts.
- Hardware performance counters.

The objective is to confirm that the chip works correctly under realistic and extreme operating conditions.

## Pre-silicon versus post-silicon

|Area|Pre-silicon verification|Post-silicon validation|
|---|---|---|
|Hardware|RTL model|Fabricated chip|
|Visibility|High internal visibility|Limited internal visibility|
|Control|Signals can often be forced|Mostly registers, firmware, and debug ports|
|Speed|Simulation can be slow|Runs near real hardware speed|
|Failures|Logic and design bugs|Logic, timing, power, PCB, firmware, and manufacturing bugs|
|Tools|Simulators, formal tools, emulators|Lab instruments, JTAG, firmware, automation|
|Reproduction|Usually deterministic|May depend on temperature, voltage, timing, or physical conditions|

## Validation versus production test

### Validation

Validation determines whether the design behaves correctly.

Examples:

- Does the camera interface support its maximum data rate?
- Does the GPU produce correct results under heavy load?
- Does the SoC boot across voltage and temperature corners?
- Does the interconnect maintain data integrity under congestion?

### Production test

Production test determines whether each manufactured chip or board has physical defects.

Examples:

- Broken connections.
- Stuck-at faults.
- Memory defects.
- Short circuits.
- Manufacturing variations.

Production tests must often run quickly because thousands or millions of devices may be tested.

## Major post-silicon stages

### 1. Initial bring-up

- Verify power rails.
- Verify clock sources.
- Verify reset behavior.
- Establish JTAG access.
- Confirm boot ROM execution.
- Obtain basic console output.
- Initialize memory.

### 2. Basic functional validation

- CPU execution.
- SRAM and DRAM.
- Interrupt controller.
- Timers.
- GPIO.
- UART, SPI, and I2C.
- Storage.
- Major accelerators.

### 3. Feature validation

- GPU functions.
- Neural-network acceleration.
- Camera interfaces.
- Security engines.
- DMA.
- High-speed interfaces.

### 4. Stress testing

- Sustained heavy workloads.
- Concurrent subsystem activity.
- High memory bandwidth.
- Repeated resets.
- Long-duration operation.
- Error injection.

### 5. Characterization

- Voltage-frequency limits.
- Temperature limits.
- Power consumption.
- Performance.
- Signal integrity.
- Process variation.

### 6. Production readiness

- Stable regression suite.
- Known limitations documented.
- Failures classified.
- Manufacturing test coverage established.
- Fixes verified.

## Common tools and what they are

- **Embedded firmware:** Low-level software that runs close to the hardware. It configures clocks, resets, memory controllers, registers, interrupt handlers, and test workloads before a full operating system is available.
- **Operating-system software:** Drivers, kernel services, and user-space tools that exercise the chip in a more realistic system environment.
- **JTAG:** A hardware debug and test interface originally standardized for boundary scan. In validation it is commonly used to detect devices, halt CPUs, inspect registers, access memory, program flash, and debug systems before normal boot works.
- **SWD:** Serial Wire Debug, a two-wire ARM debug interface that provides many debug capabilities similar to JTAG using fewer pins. It is common on ARM microcontrollers and some ARM-based SoCs.
- **Oscilloscope:** A measurement instrument that shows voltage versus time. Use it for clocks, resets, power-rail droop, analog behavior, and signal-quality checks.
- **Logic analyzer:** A digital instrument that captures high/low transitions across many channels. Use it for I2C, SPI, UART, GPIO, reset, interrupt, and simple protocol timing.
- **Protocol analyzer:** A tool that understands a specific protocol such as PCIe, USB, Ethernet, or MIPI and decodes packets, link states, errors, and timing.
- **Bit-error-rate tester:** Equipment or built-in hardware that transmits known bit patterns and counts incorrect received bits. It is used heavily for SerDes links.
- **Thermal chamber:** A controlled environment that runs hardware at hot and cold temperature corners.
- **Hardware performance counter:** A register or counter inside the SoC that counts events such as cycles, cache misses, interrupts, memory transactions, stalls, or protocol errors.

## Why post-silicon bugs are difficult

Post-silicon failures often have limited visibility. In pre-silicon simulation, an engineer may inspect almost any internal signal. On real hardware, visibility usually comes from logs, registers, counters, external pins, debug ports, and instruments. This makes isolation more important than guessing.

Failures can also be multi-factor. A camera failure might be caused by a camera sensor, MIPI link, DMA descriptor, memory bandwidth conflict, interrupt latency, power droop, or firmware timing bug. Good validation breaks the system into layers and proves which layers are working.

---
