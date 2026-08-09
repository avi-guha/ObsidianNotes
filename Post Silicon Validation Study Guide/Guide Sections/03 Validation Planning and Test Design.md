# 3. Validation Planning and Test Design

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/02 Post-Silicon Validation Fundamentals|Previous: 2. Post-Silicon Validation Fundamentals]] | [[Guide Sections/04 Computer Architecture|Next: 4. Computer Architecture]]

## Components of a strong validation plan

Each test plan should contain:

### Objective

What requirement or behavior is being validated?

### Scope

Which block, interface, board, firmware version, and operating mode are included?

### Requirements

What specification defines correct behavior?

### Setup

- Hardware revision.
- Firmware version.
- Test equipment.
- Cabling.
- Environmental conditions.
- Power configuration.
- Software dependencies.

### Stimulus

What workload, command, data pattern, or event will be applied?

### Expected result

What should happen?

### Pass/fail criteria

The criterion should be measurable.

Weak criterion:

> The interface should work properly.

Strong criterion:

> The link must complete training within 500 ms and transmit (10^{12}) bits without an uncorrected error.

### Corner cases

- Minimum and maximum clock frequency.
- Minimum and maximum voltage.
- Low and high temperature.
- Minimum and maximum packet size.
- Empty and full buffers.
- Maximum number of concurrent requests.
- Reset during active traffic.
- Repeated startup and shutdown.
- Error conditions.

### Observability

What will be recorded?

- Register values.
- Error counters.
- Firmware logs.
- Performance counters.
- Oscilloscope captures.
- Power measurements.
- Temperature.
- Test duration.
- Device identifiers.
- Software versions.

### Reproduction procedure

A failure should be reproducible by another engineer using the recorded setup.

## Test categories

### Directed test

Targets a specific behavior.

Example:

> Verify that a camera frame-completion interrupt is generated after one frame.

### Stress test

Pushes a resource close to its limits.

Example:

> Run all camera interfaces while the GPU and NPU generate maximum memory traffic.

### Negative test

Applies invalid or unexpected input.

Example:

> Send an unsupported packet length and verify that the interface reports an error without crashing.

### Corner test

Exercises operating boundaries.

Example:

> Boot at the lowest supported voltage and highest supported temperature.

### Regression test

Runs repeatedly after changes to detect newly introduced problems.

### Soak test

Runs for a long duration to expose rare failures, leaks, drift, or thermal effects.

### Characterization test

Measures behavior over a range rather than producing only pass or fail.

Example:

> Sweep voltage and frequency to map the stable operating region.

## Validation coverage

Coverage answers:

> What have we tested, and what remains untested?

Useful coverage dimensions:

- Feature coverage.
- Operating-mode coverage.
- Voltage coverage.
- Frequency coverage.
- Temperature coverage.
- Data-pattern coverage.
- Error-condition coverage.
- Concurrency coverage.
- Reset and recovery coverage.
- Hardware-revision coverage.
- Firmware-version coverage.

## Example test-plan entry

### Test: GPU and camera memory-contention stress

**Objective:** Verify correct camera capture while the GPU generates high memory-bandwidth traffic.

**Setup:**

- Production-like board.
- Four cameras enabled.
- Maximum supported frame rate.
- GPU memory benchmark.
- Performance counters enabled.
- Thermal and power logging enabled.

**Procedure:**

1. Boot the system.
2. Confirm all camera links are stable.
3. Start camera capture.
4. Record five minutes of baseline behavior.
5. Start the GPU memory workload.
6. Run for two hours.
7. Monitor dropped frames, memory errors, GPU output, temperature, clocks, and rail power.
8. Repeat across multiple boards.

**Pass criteria:**

- No corrupted frames.
- Frame-drop rate below the specified limit.
- No uncorrected memory errors.
- GPU results match the reference output.
- No unexplained system resets.
- No thermal or power-limit violation.

## Terms that make a test plan stronger

- **Requirement:** A specific behavior the design must satisfy. Requirements should come from specifications, architecture documents, interface standards, or agreed product targets.
- **Stimulus:** The input applied to the system. It can be a command, packet stream, workload, voltage sweep, reset event, camera frame pattern, or software operation.
- **Expected result:** The behavior that proves the requirement is met. This should be explicit enough that another engineer would reach the same pass/fail decision.
- **Pass/fail criteria:** The measurable threshold for success. Good criteria include numbers, limits, durations, error counts, tolerances, and required logs or counters.
- **Corner case:** A boundary condition such as minimum voltage, maximum temperature, maximum packet size, empty buffers, full queues, reset during traffic, or unsupported input.
- **Observability:** The data available to understand what happened. Logs alone are often insufficient; register dumps, counters, waveforms, timestamps, versions, and equipment state matter.
- **Reproducibility:** The ability for another engineer or automated system to run the same test and see the same result under the same conditions.

## How to avoid weak validation plans

A weak plan says only what will be exercised. A strong plan says what correct behavior means, how the test will detect incorrect behavior, and what evidence will be saved.

For example, "test SPI" is weak. A stronger version is: "At 1 MHz, 10 MHz, and the maximum supported SPI clock, transfer fixed, walking-bit, and pseudorandom payloads of 1, 16, 256, and 4096 bytes. Verify exact data match, no timeout, no unexpected status flags, and correct chip-select timing on a logic analyzer."

When a test fails, the plan should already contain enough information to classify the failure as a device failure, test bug, setup problem, equipment problem, or inconclusive result.

---
