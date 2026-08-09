# 19. Debugging Framework

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/18 AI-Assisted Validation|Previous: 18. AI-Assisted Validation]] | [[Guide Sections/20 Behavioral Interview Preparation|Next: 20. Behavioral Interview Preparation]]

## Layered debugging

For a system that is not working, divide the problem into layers.

### Hardware layer

- Power.
- Clock.
- Reset.
- Wiring.
- Signal integrity.
- Component assembly.

### Transport layer

- I2C.
- SPI.
- UART.
- PCIe.
- Ethernet.
- MIPI.
- JTAG.

### Configuration layer

- Registers.
- Boot straps.
- Clock settings.
- Pin multiplexing.
- Interrupt setup.

### Firmware layer

- Driver logic.
- State machine.
- Timing.
- Buffers.
- Memory management.
- Concurrency.

### Application layer

- Data parsing.
- Algorithms.
- Workload behavior.
- Expected output.

## Debugging questions

1. What exactly is the observed symptom?
2. What is the expected behavior?
3. Is the failure reproducible?
4. What changed recently?
5. What is the simplest failing case?
6. Which layers are confirmed working?
7. What evidence supports each hypothesis?
8. What single experiment best distinguishes the hypotheses?
9. Does the failure follow the hardware, software, or environment?
10. What data must be collected before resetting?

## Binary-search debugging

Binary search can be applied to:

- Firmware commits.
- Workload size.
- Frequency.
- Voltage.
- Number of active subsystems.
- Test duration.
- Address range.
- Data pattern.
- Temperature.

## A/B swap method

Swap one component between good and bad systems.

Examples:

- Cable.
- SoC board.
- Power supply.
- Camera.
- Memory module.
- Firmware image.

Observe whether the failure follows the swapped item.

## Debugging statement template

> I first clarified the exact failure and established a reproducible test. I divided the system into physical, communication, configuration, firmware, and application layers. I added observability at each boundary, compared against a known-good configuration, and changed one variable at a time. After reducing the failure to the smallest case, I verified the root cause with a targeted experiment and added a regression test.

## Layer terms in more detail

- **Hardware layer:** The physical conditions required for anything else to work: power, clocks, reset, wiring, connectors, soldering, signal integrity, and component orientation.
- **Transport layer:** The communication path and protocol between devices, such as I2C, SPI, UART, PCIe, Ethernet, MIPI, JTAG, or USB.
- **Configuration layer:** Register settings, straps, pin muxes, clock muxes, interrupt routing, memory maps, and security settings that determine how hardware behaves.
- **Firmware layer:** The low-level software that sequences hardware, manages state, handles interrupts, configures DMA, and exposes logs or commands.
- **Application layer:** Higher-level parsing, algorithms, workloads, expected output, and user-visible behavior.

## Debugging tools by layer

- **Hardware:** Multimeter, oscilloscope, current-limited supply, thermal camera, microscope, schematic, layout viewer, and golden-board comparison.
- **Transport:** Logic analyzer, protocol analyzer, bus scanner, loopback test, known-good cable, and error counters.
- **Configuration:** Register dump, boot strap check, firmware version, clock tree dump, pinmux dump, and reset-reason register.
- **Firmware:** Serial logs, breakpoints, JTAG/SWD halt, trace buffer, watchdog records, assertions, and state-machine markers.
- **Application:** Input capture, reference output, checksums, packet logs, performance counters, and reduced test cases.

## Good hypothesis table

Use a small table when debugging gets confusing:

|Hypothesis|Evidence for|Evidence against|Next experiment|
|---|---|---|---|
|Power droop during GPU burst|Reset occurs at workload start|Idle and CPU-only pass|Scope core rail during burst|
|Firmware race|Failure rate changes with logging|Rail looks stable|Add lock/assertion and run stress|
|Camera DMA overflow|Dropped frames plus FIFO full|MIPI CRC count is zero|Reduce memory traffic and compare|

---
