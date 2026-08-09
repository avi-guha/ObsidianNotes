# 23. Project Story Templates

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/22 Coding Practice|Previous: 22. Coding Practice]] | [[Guide Sections/24 Two-Week Study Plan|Next: 24. Two-Week Study Plan]]

## GPS and telemetry debugging story

### Situation

I was integrating a u-blox GPS module with an STM32-based telemetry system using I2C.

### Task

I needed to establish reliable communication, configure the receiver, obtain valid navigation messages, and confirm operation on both a development board and the target telemetry board.

### Action

I separated the problem into physical wiring, I2C transport, device configuration, data availability, parsing, and satellite-fix layers.

I:

- Verified the device address.
- Checked STM32 HAL return codes.
- Inspected I2C error status.
- Corrected pin-mapping assumptions.
- Added startup delays.
- Added finite read timeouts.
- Confirmed configuration acknowledgements.
- Inspected raw receive buffers.
- Distinguished successful bus communication from valid NMEA output.
- Used J-Link during target-board debugging.
- Compared behavior across development and target hardware.

### Result

I established which layers were functioning and reduced the remaining problem to message-output and timing behavior rather than treating the entire GPS system as one failure.

### Lesson

A successful low-level communication status does not guarantee that the full application pipeline works. Every interface boundary needs its own observable success criterion.

## Power-delivery story

Use this structure:

- What rail or regulator was being analyzed?
- What were the current and transient requirements?
- How did you select decoupling?
- What measurements or calculations did you use?
- What tradeoffs existed?
- How did you verify stability and margin?

## Verilog or FPGA story

Discuss:

- Requirements.
- State-machine design.
- Reset behavior.
- Timing.
- Simulation.
- Corner cases.
- Synthesis.
- Hardware verification.
- Bugs found and corrected.

## Robotics story

Discuss:

- Sensors and actuators.
- Firmware architecture.
- Communication interfaces.
- Calibration.
- Real-time constraints.
- Integration failures.
- Physical-system debugging.
- Safety and recovery behavior.

## Neural-network project story

Connect it to the role through:

- Tensor workloads.
- Performance measurement.
- Accuracy comparison.
- Data preprocessing.
- Determinism.
- Compute versus memory cost.
- Hardware-accelerator interest.

## How to make each story validation-relevant

Tie every story back to layers, observability, and measurable outcomes. Even a class project can sound relevant if you explain how you verified assumptions, handled edge cases, debugged hardware/software boundaries, and made the result reproducible.

- **GPS story:** Emphasize I2C transport versus application-level NMEA output. A successful bus transaction proves communication with the device, not necessarily valid navigation data.
- **Power story:** Emphasize rails, transient current, decoupling, regulator limits, measurement points, and verification under load.
- **FPGA story:** Emphasize state machines, reset behavior, simulation, timing closure, hardware observation, and how you found mismatches between simulation and hardware.
- **Robotics story:** Emphasize sensors, actuators, timing, safety, calibration, physical noise, and recovery from bad states.
- **Neural-network story:** Emphasize reference outputs, tolerance, performance counters, determinism, data movement, and compute versus memory bottlenecks.

## Story checklist

Before using a story in an interview, make sure you can answer:

1. What exactly was failing or uncertain?
2. What evidence did you collect?
3. What hypotheses did you consider?
4. What did you personally implement or measure?
5. What was the measurable result?
6. What would you do differently now?

---
