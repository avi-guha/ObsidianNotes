# 21. Technical Interview Questions

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/20 Behavioral Interview Preparation|Previous: 20. Behavioral Interview Preparation]] | [[Guide Sections/22 Coding Practice|Next: 22. Coding Practice]]

## Post-silicon

1. What is post-silicon validation?
2. How is it different from pre-silicon verification?
3. How would you validate a newly fabricated SoC?
4. What belongs in a validation plan?
5. How do you define pass/fail criteria?
6. What is a corner test?
7. What is a soak test?
8. How do you measure validation coverage?
9. How do you make a failure reproducible?
10. How do you distinguish a device failure from a test-infrastructure failure?

## Architecture

1. Explain a CPU pipeline.
2. What are pipeline hazards?
3. What is branch prediction?
4. What is a cache miss?
5. What is cache coherence?
6. What is a TLB?
7. What is a page fault?
8. Explain DMA.
9. What is memory-mapped I/O?
10. Why are memory barriers needed?
11. What is false sharing?
12. What is the difference between latency and throughput?
13. How do you identify a memory-bound workload?
14. What is an atomic operation?
15. What happens when an interrupt occurs?

## SoC and fabric

1. What is an SoC interconnect?
2. What are initiators and targets?
3. What is arbitration?
4. What is backpressure?
5. What is quality of service?
6. What is starvation?
7. What is deadlock?
8. How would you stress the interconnect?
9. How would you validate coherency?
10. How could camera traffic be affected by GPU traffic?

## SerDes

1. What is a SerDes?
2. What is differential signaling?
3. What is clock and data recovery?
4. What is jitter?
5. What does an eye diagram show?
6. What is BER?
7. What is PRBS?
8. What is equalization?
9. What is loopback?
10. How would you validate a SerDes?
11. Why might a link work at low speed but fail at high speed?
12. How would you isolate a bad lane?

## Power

1. Explain (P \approx \alpha C V^2 f).
2. What is static power?
3. What is DVFS?
4. What is clock gating?
5. What is power gating?
6. What is voltage droop?
7. How do you measure current using a shunt resistor?
8. Why do transient measurements matter?
9. What causes thermal throttling?
10. How would you identify which block causes excess power?

## Embedded systems

1. What does `volatile` mean?
2. Is `volatile` atomic?
3. What is read-modify-write?
4. What is an ISR?
5. Polling versus interrupts?
6. What is a race condition?
7. Stack versus heap?
8. What is endianness?
9. What is struct padding?
10. How do you safely poll a register?
11. Why must tests have timeouts?
12. What is a watchdog?
13. How do you debug memory corruption?
14. What happens if a DMA buffer is cached?
15. How do you prevent an ISR from doing too much work?

## Board bring-up

1. A new board does not boot. What do you check?
2. Why use a current-limited supply?
3. How do you verify rail sequencing?
4. What can JTAG tell you before firmware boots?
5. How do you identify the last successful boot stage?
6. Why use a golden board?
7. What is a boot strap?
8. How do you determine whether a clock is valid?
9. What does power-good mean?
10. How would you debug a board that resets under load?

## Automation

1. Design a validation framework for 20 boards.
2. How do you distinguish test failures from infrastructure failures?
3. What data should be logged?
4. How should retries be handled?
5. What should happen when a board freezes?
6. How do you make tests reproducible?
7. How do you reserve shared equipment?
8. How do you detect flaky tests?
9. What makes a good test API?
10. How would you store and compare regression data?

## Failure analysis

1. How do you investigate a field return?
2. Why preserve the original state?
3. What is an intermittent failure?
4. How do you isolate a temperature-dependent issue?
5. What is the difference between a symptom and root cause?
6. How do you prove a fix?
7. What is an A/B swap?
8. How do you use binary search in debugging?
9. How do you investigate a failure that occurs once every 12 hours?
10. How do you determine whether a bug is in silicon, PCB, or firmware?

## How to answer technical questions

For definition questions, give a one-sentence definition, then explain why it matters in validation. For debug questions, start with assumptions, then move from physical fundamentals to software behavior. For design questions, define measurable pass/fail criteria and what data you would collect.

Good answer pattern:

1. Define the concept.
2. State why it matters.
3. Give a concrete example.
4. Mention how you would measure, validate, or debug it.

Example:

> A TLB is a cache of virtual-to-physical address translations. It matters because TLB misses add latency and can make large or irregular memory workloads slower. In validation, I would look at TLB-miss counters, vary working-set size and page size, and compare performance scaling against cache and memory-bandwidth counters.

## Compact answer cues

- **Post-silicon validation:** Real-chip testing across function, performance, power, corners, and system interactions.
- **Corner test:** A boundary-condition test, such as low voltage plus high temperature or maximum packet size plus reset.
- **Soak test:** A long-duration run used to expose rare, thermal, leakage, drift, or resource-leak failures.
- **DMA:** Hardware data movement without CPU copying every word; watch for cache coherence, alignment, descriptors, and lifetime.
- **MMIO:** Hardware registers mapped into memory space; use volatile access and required barriers.
- **Backpressure:** A receiver cannot accept more traffic and tells upstream logic to slow or stop.
- **Deadlock:** Blocks wait on each other with no forward progress.
- **Eye diagram:** Overlaid serial bits showing voltage and timing margin.
- **DVFS:** Runtime voltage/frequency adjustment used to balance performance, power, and thermal limits.
- **Watchdog:** Timer that detects lack of progress and triggers reset or recovery.
- **Infrastructure failure:** The test setup failed, not necessarily the device.

---
