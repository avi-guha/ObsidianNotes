# 10. Board and Silicon Bring-Up

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/09 Performance Characterization|Previous: 9. Performance Characterization]] | [[Guide Sections/11 Embedded C and C++|Next: 11. Embedded C and C++]]

## Bring-up philosophy

Do not begin with complex software.

Start with physical fundamentals and move upward through layers.

```text
Power
↓
Clocks
↓
Reset
↓
Debug access
↓
Boot ROM
↓
Memory
↓
Firmware
↓
Peripherals
↓
Operating system
↓
Applications and stress tests
```

## Board bring-up checklist

### Before power

- Verify board revision.
- Review the schematic.
- Inspect component orientation.
- Inspect soldering and connectors.
- Measure resistance from major rails to ground.
- Check for visible shorts or damage.
- Verify jumper and strap configuration.
- Confirm the correct power supply and current limit.

### Initial power-on

- Use a current-limited source.
- Observe input current.
- Stop if current is unexpectedly high.
- Verify all power rails.
- Verify rail sequence.
- Check regulator enable signals.
- Check power-good signals.
- Monitor component temperatures.

### Clock and reset

- Verify crystal or oscillator activity.
- Verify reference-clock frequency.
- Verify reset assertion.
- Verify reset release.
- Confirm reset timing relative to power and clocks.

### Debug access

- Confirm JTAG chain detection.
- Read device ID.
- Halt the CPU.
- Read CPU registers.
- Read and write memory.
- Verify the reset vector.
- Program firmware.

### Boot

- Confirm boot straps.
- Confirm boot source.
- Confirm boot ROM execution.
- Obtain UART logs.
- Identify the last successful boot stage.
- Verify external memory initialization.
- Confirm firmware image integrity.

### Peripheral bring-up

Bring up one subsystem at a time:

- UART.
- GPIO.
- Timers.
- I2C.
- SPI.
- Storage.
- DRAM.
- Ethernet.
- Cameras.
- GPU.
- NPU.
- High-speed interfaces.

## Debugging a board that does not boot

Ask:

1. Is correct input power present?
2. Is input current reasonable?
3. Are all required rails present?
4. Is sequencing correct?
5. Are clocks active?
6. Is reset released?
7. Are boot straps correct?
8. Can JTAG detect the target?
9. Can the CPU be halted?
10. Is the CPU reaching the reset vector?
11. Is boot ROM running?
12. Is external memory initialized?
13. Is the firmware image valid?
14. Is there UART output?
15. Does the problem follow the board or the SoC?

## Golden-board comparison

A golden board is a known-good reference unit.

Compare:

- Rail voltages.
- Current draw.
- Clock waveforms.
- Reset timing.
- Register values.
- Boot logs.
- Firmware version.
- Temperature.
- Interface activity.

## Bring-up terms in more detail

- **Power rail:** A named supply voltage feeding a block, such as CPU core, memory, I/O, analog, or always-on logic.
- **Sequencing:** The required order and timing for rails, clocks, and resets. Some devices can latch incorrect states or be damaged if sequencing is wrong.
- **Current limit:** A bench-supply setting that prevents excessive current during first power-on. It can protect the board while you detect shorts or assembly problems.
- **Power-good signal:** A regulator or supervisor output indicating that a rail is within its valid range.
- **Reset assertion:** Holding reset active so logic remains in a known state. Reset release should occur only after required rails and clocks are stable.
- **Boot strap:** A hardware pin or resistor setting sampled at reset to choose boot mode, boot source, debug mode, voltage mode, or device configuration.
- **Reset vector:** The first address the CPU fetches after reset. Reading or observing it helps determine whether the CPU is beginning execution correctly.
- **Boot ROM:** Mask-programmed code inside the SoC that runs before external firmware. It often configures minimal hardware and loads the next boot stage.
- **Golden board:** A known-good unit used as a baseline for measurements and register comparisons.

## Practical no-boot isolation

If a board does not boot, split the question into "is the silicon alive?" and "is software progressing?" JTAG device ID, halt, register reads, and memory access can prove basic debug connectivity even when UART is silent. UART output proves firmware reached a logging stage, but silence does not prove the CPU never ran; UART pins, muxing, baud rate, clock configuration, and firmware logging may also be wrong.

---
