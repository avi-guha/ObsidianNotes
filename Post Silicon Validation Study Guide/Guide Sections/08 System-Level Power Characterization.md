# 8. System-Level Power Characterization

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/07 High-Speed SerDes Validation|Previous: 7. High-Speed SerDes Validation]] | [[Guide Sections/09 Performance Characterization|Next: 9. Performance Characterization]]

## Dynamic power

A simplified CMOS power relationship is:

[
P_{\text{dynamic}} \approx \alpha C V^2 f
]

Where:

- (\alpha) = switching activity.
- (C) = effective capacitance.
- (V) = supply voltage.
- (f) = frequency.

Important implication:

> Dynamic power increases approximately with the square of voltage.

## Static power

Static power is largely caused by leakage current.

[
P_{\text{static}} = V I_{\text{leakage}}
]

Leakage often increases with temperature.

## Total power

[
P_{\text{total}} = P_{\text{dynamic}} + P_{\text{static}}
]

## Power-management techniques

### Clock gating

Stops the clock to inactive logic, reducing switching.

### Power gating

Disconnects power from inactive blocks, reducing leakage.

### DVFS

Dynamic Voltage and Frequency Scaling adjusts voltage and frequency according to performance demand.

### Thermal throttling

Reduces performance when temperature exceeds a limit.

## Rail-level measurements

A SoC may have separate rails for:

- CPU cores.
- GPU.
- NPU.
- Memory.
- SerDes.
- I/O.
- PLL or analog circuits.
- Always-on logic.

Measuring only total board power may hide which subsystem causes a change.

## Measuring current with a shunt resistor

[
I = \frac{V_{\text{shunt}}}{R_{\text{shunt}}}
]

Then:

[
P = VI
]

## Measurement concerns

- Instrument bandwidth.
- Sampling rate.
- Shunt resistance.
- Measurement noise.
- Probe loading.
- Calibration.
- Grounding.
- Averaging window.
- Transient versus steady-state power.
- Synchronization with workload phases.

## Power characterization plan

1. Measure powered-off leakage if relevant.
2. Measure idle power.
3. Measure boot power.
4. Measure one block at a time.
5. Measure combined workloads.
6. Capture average power.
7. Capture peak power.
8. Capture transient response.
9. Record temperatures.
10. Record clock frequencies.
11. Sweep voltage and frequency.
12. Repeat across multiple units.
13. Compare board revisions.
14. Identify unexpected rail behavior.

## Voltage droop

Voltage droop occurs when a sudden increase in current causes a temporary reduction in supply voltage.

Potential effects:

- Timing failures.
- Logic errors.
- System reset.
- PLL unlock.
- SerDes errors.
- Memory corruption.

Possible causes:

- Insufficient decoupling.
- Regulator response limitations.
- Excessive path resistance or inductance.
- Large workload transitions.
- Weak power-delivery network.

## Example power-debug question

> Performance is lower than expected while power is high. What would you investigate?

A strong answer:

1. Confirm the test and software versions.
2. Check whether the workload is correct.
3. Record clock frequencies.
4. Check thermal throttling.
5. Measure major power rails.
6. Check CPU, GPU, NPU, and memory utilization.
7. Inspect cache misses and memory bandwidth.
8. Check fabric contention.
9. Compare against a known-good board.
10. Sweep workload size.
11. Determine whether the bottleneck is compute, memory, software, power delivery, or temperature.

## Power terms in more detail

- **Dynamic power:** Power consumed by switching transistors and charging/discharging capacitance. It rises with activity, capacitance, frequency, and especially voltage.
- **Static power:** Power consumed even when logic is not actively switching, mostly from leakage. Leakage usually increases strongly with temperature.
- **Clock gating:** Stops clocks to idle logic. It reduces dynamic power but does not remove leakage.
- **Power gating:** Turns off power to an idle block. It reduces leakage but requires state save/restore, isolation, and wake-up sequencing.
- **DVFS:** Dynamic Voltage and Frequency Scaling. Lower frequency may allow lower voltage, and lower voltage has a large power benefit because dynamic power scales roughly with V squared.
- **Thermal throttling:** Automatic performance reduction to keep temperature below a limit. It can make performance results look inconsistent unless temperature and clocks are logged.
- **Voltage droop:** A short supply-voltage dip after a sudden load increase. Droop is often more important than average voltage because timing failures can occur during the transient.

## Measurement detail that matters

A power number without context is weak. Record the rail name, sense location, shunt value, sample rate, averaging window, workload phase, clock frequency, voltage, temperature, board revision, firmware version, and whether the result is peak, average, or steady state.

For fast workload transitions, a slow digital multimeter may miss the important event. Use an oscilloscope, fast power monitor, or instrumented regulator telemetry when transient droop, inrush current, or workload burst power matters.

---
