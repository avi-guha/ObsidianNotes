# 16. Failure Analysis and Field Returns

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/15 Automated Validation Infrastructure|Previous: 15. Automated Validation Infrastructure]] | [[Guide Sections/17 Design-for-Test Concepts|Next: 17. Design-for-Test Concepts]]

## Failure-analysis objective

Determine the true root cause of a failure and prevent recurrence.

Potential root-cause categories:

- Silicon design.
- Silicon manufacturing.
- Package.
- PCB design.
- PCB assembly.
- Component defect.
- Power delivery.
- Signal integrity.
- Firmware.
- Driver.
- Operating system.
- Application software.
- Test infrastructure.
- Environmental conditions.
- Mechanical damage.

## Failure-analysis process

### 1. Preserve evidence

- Do not immediately reflash or reset.
- Record the returned condition.
- Photograph the unit if relevant.
- Record serial number and revision.
- Save logs and crash dumps.
- Record customer or manufacturing symptoms.

### 2. Reproduce

Determine:

- Frequency.
- Trigger.
- Workload.
- Temperature.
- Voltage.
- Time dependence.
- Boot-cycle dependence.
- Software-version dependence.

### 3. Compare

Use:

- Known-good board.
- Known-good SoC.
- Same board revision.
- Same firmware.
- Same test equipment.

### 4. Isolate

Change one variable at a time:

- Board.
- SoC.
- Cable.
- Power supply.
- Firmware.
- Temperature.
- Workload.
- Peripheral.
- Memory.
- Clock.

### 5. Minimize

Reduce the failure to:

- The smallest workload.
- The fewest active blocks.
- The shortest reproduction time.
- The simplest software.
- The minimum environmental condition.

### 6. Instrument

Collect:

- Register snapshots.
- Error counters.
- Rail waveforms.
- Clock waveforms.
- Memory dumps.
- Protocol traces.
- Thermal data.
- Performance counters.

### 7. Form and test hypotheses

For each hypothesis, define:

- Supporting evidence.
- Contradicting evidence.
- Experiment.
- Expected result.

### 8. Verify the fix

A fix is not complete until:

- The original failure no longer occurs.
- The correction does not introduce new failures.
- Multiple units pass.
- Relevant corner conditions pass.
- A regression test is added.

## Intermittent failures

Intermittent failures may depend on:

- Temperature.
- Voltage.
- Timing.
- Noise.
- Mechanical pressure.
- Manufacturing variation.
- Race condition.
- Rare data pattern.
- Long operating duration.

Strategies:

- Increase repetition.
- Log exact conditions.
- Automate reproduction.
- Sweep one variable at a time.
- Use binary search.
- Compare multiple units.
- Trigger captures around the failure.

## Symptom versus root cause

Example:

**Symptom:** Camera frames are dropped.

Possible root causes:

- Camera interface error.
- DMA bug.
- Memory congestion.
- Buffer-size problem.
- Firmware interrupt delay.
- Power droop.
- Thermal throttling.
- Incorrect QoS configuration.

Do not stop at the symptom.

## Failure-analysis terms in more detail

- **Symptom:** What was observed, such as "no boot," "frame drop," "high current," or "link errors." A symptom is not the same as the cause.
- **Root cause:** The underlying reason the failure occurred. It should explain the evidence and predict the result of a fix.
- **Field return:** A unit returned from real customer or deployed use. Preserve its state because the original condition may be hard to recreate.
- **Crash dump:** Saved processor, memory, or software state captured after a fault. It can include registers, stack traces, logs, and memory regions.
- **Intermittent failure:** A failure that does not happen every time. It often depends on temperature, voltage, timing, noise, mechanical stress, data pattern, or race conditions.
- **A/B swap:** Swapping one variable between a passing system and failing system to see whether the failure follows the item.
- **Hypothesis:** A proposed explanation that can be tested with an experiment. A good hypothesis includes what result would support it and what result would disprove it.
- **Regression test:** A test added after a fix to make sure the same class of bug is not reintroduced.

## Evidence quality

Strong evidence is specific, timestamped, and tied to a controlled setup. "It failed after a while" is weak. "Board A17 running firmware `abc123` at 0.82 V core, 95 C chamber temperature, four-camera capture plus GPU memory stress, failed after 47 minutes with camera FIFO overflow and DRAM ECC counter increment" is useful.

When possible, preserve the failing condition before applying recovery. A power cycle, reflash, or reset may erase the only evidence that distinguishes firmware crash, power fault, watchdog reset, or hardware lockup.

---
