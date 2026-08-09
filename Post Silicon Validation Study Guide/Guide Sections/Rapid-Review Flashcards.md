# Rapid-Review Flashcards

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/25 Final Interview Cheat Sheet|Previous: 25. Final Interview Cheat Sheet]] | [[Guide Sections/Final Preparation Checklist|Next: Final Preparation Checklist]]

## Card 1

**Q:** What is post-silicon validation?
**A:** Testing a fabricated chip in real hardware to verify functionality, performance, power, interfaces, corner behavior, and system interactions.

## Card 2

**Q:** What is the core debugging loop?
**A:** Observe, measure, isolate, reproduce, minimize, fix, verify, and regress.

## Card 3

**Q:** What does an eye diagram show?
**A:** Timing and voltage margin of a serial signal across many overlaid bit periods.

## Card 4

**Q:** What is BER?
**A:** Incorrect received bits divided by total transmitted bits.

## Card 5

**Q:** Why use PRBS?
**A:** To generate repeatable, high-transition serial data that stresses a link.

## Card 6

**Q:** What is voltage droop?
**A:** A temporary supply-voltage reduction caused by a rapid increase in load current.

## Card 7

**Q:** What is the dynamic-power relationship?
**A:** (P \approx \alpha C V^2 f).

## Card 8

**Q:** What is DMA?
**A:** Hardware that transfers data between memory and devices without the CPU copying each word.

## Card 9

**Q:** What is backpressure?
**A:** A receiver slowing or stopping incoming traffic because it cannot accept more data.

## Card 10

**Q:** What does `volatile` provide?
**A:** It prevents the compiler from assuming a value remains unchanged; it does not provide atomicity.

## Card 11

**Q:** Why are timeouts critical?
**A:** They prevent one hung operation from blocking an entire validation regression.

## Card 12

**Q:** What is a golden board?
**A:** A known-good board used as a comparison baseline.

## Card 13

**Q:** What is a soak test?
**A:** A long-duration test intended to expose rare or time-dependent failures.

## Card 14

**Q:** What is MBIST?
**A:** On-chip hardware that tests embedded memories.

## Card 15

**Q:** What should happen after a bug is fixed?
**A:** Verify the fix across relevant conditions and add a regression test.

## Card 16

**Q:** What is SWD?
**A:** Serial Wire Debug, a low-pin-count ARM debug interface used for halt, register access, memory access, and breakpoints.

## Card 17

**Q:** What is OpenOCD?
**A:** Host software that connects a debug probe to a target through JTAG or SWD and can expose a GDB server.

## Card 18

**Q:** What is QoS in an SoC fabric?
**A:** Quality of Service policy that prioritizes traffic so latency-sensitive agents, such as cameras, meet deadlines while other traffic still progresses.

## Card 19

**Q:** What is an infrastructure error?
**A:** A test failure caused by the environment, equipment, host, configuration, or automation rather than the device under test.

## Card 20

**Q:** What is a boot strap?
**A:** A hardware pin or resistor setting sampled at reset to select boot mode, boot source, or related configuration.

## Card 21

**Q:** Why can DMA cause cache-coherency bugs?
**A:** The CPU and device may see different versions of a buffer unless software uses coherent mappings, cache maintenance, or barriers.

## Card 22

**Q:** What does a logic analyzer show?
**A:** Digital high/low transitions over time, useful for interfaces such as I2C, SPI, UART, GPIO, reset, and interrupts.

---
