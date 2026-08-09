# 15. Automated Validation Infrastructure

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/14 Schematics and PCB Review|Previous: 14. Schematics and PCB Review]] | [[Guide Sections/16 Failure Analysis and Field Returns|Next: 16. Failure Analysis and Field Returns]]

## Goals

A validation framework should be:

- Reproducible.
- Scalable.
- Reliable.
- Observable.
- Recoverable.
- Version-controlled.
- Easy to extend.
- Safe for hardware.

## Typical architecture

```text
Test scheduler
    ↓
Python test runner
    ↓
Board-control layer
    ├── Serial
    ├── SSH
    ├── JTAG
    ├── Network API
    └── Debug interface
    ↓
Instrument-control layer
    ├── Power supply
    ├── Oscilloscope
    ├── Power monitor
    ├── Thermal chamber
    └── Protocol analyzer
    ↓
Results and artifacts
    ├── Logs
    ├── Measurements
    ├── Register dumps
    ├── Waveforms
    └── Database
```

## Framework components

### Test definition

Contains:

- Name.
- Purpose.
- Preconditions.
- Inputs.
- Steps.
- Assertions.
- Timeout.
- Cleanup.
- Required equipment.

### Device abstraction

The test should not contain every low-level serial command.

Example:

```python
board.power_on()
board.flash_firmware(image)
board.boot()
board.run_workload("gpu_stress")
results = board.read_counters()
```

### Logging

Record:

- Timestamp.
- Board serial number.
- SoC identifier.
- Hardware revision.
- Firmware commit.
- Test name.
- Test parameters.
- Equipment versions.
- Environment.
- Full output.
- Failure reason.

### Timeouts

Every external operation should have a timeout:

- Boot.
- Serial command.
- JTAG connection.
- Workload completion.
- Instrument response.
- File transfer.

### Cleanup

After every test:

- Stop workloads.
- Release instruments.
- Close ports.
- Restore board configuration.
- Reset hardware when needed.
- Preserve failure artifacts.

### Recovery

A test rack should recover from:

- Frozen firmware.
- Lost serial connection.
- Failed flash.
- Network timeout.
- Power fault.
- Crashed test process.

Potential recovery sequence:

1. Attempt software reset.
2. Attempt debug reset.
3. Power-cycle the board.
4. Reflash firmware.
5. Mark the unit unavailable if recovery fails.

## Test result categories

### Pass

The device met all requirements.

### Fail

The device violated a requirement.

### Infrastructure error

The test could not execute correctly because of the test environment.

Examples:

- Instrument disconnected.
- Serial adapter missing.
- Host storage full.

### Inconclusive

The available data is insufficient to classify the device.

## Python test example

```python
from dataclasses import dataclass
from typing import Protocol

class Board(Protocol):
    def reset(self) -> None:
        ...

    def wait_for_boot(self, timeout_s: float) -> bool:
        ...

    def run_workload(self, name: str, timeout_s: float) -> dict:
        ...

@dataclass
class TestResult:
    passed: bool
    message: str
    metrics: dict

def run_gpu_stress_test(board: Board) -> TestResult:
    board.reset()

    if not board.wait_for_boot(timeout_s=30.0):
        return TestResult(
            passed=False,
            message="Board failed to boot within 30 seconds.",
            metrics={},
        )

    try:
        metrics = board.run_workload(
            name="gpu_stress",
            timeout_s=600.0,
        )
    except TimeoutError:
        return TestResult(
            passed=False,
            message="GPU workload timed out.",
            metrics={},
        )

    error_count = int(metrics.get("error_count", -1))

    if error_count != 0:
        return TestResult(
            passed=False,
            message=f"GPU workload reported {error_count} errors.",
            metrics=metrics,
        )

    return TestResult(
        passed=True,
        message="GPU stress test passed.",
        metrics=metrics,
    )
```

## Testing 20 boards

Discuss:

- Unique board identities.
- Independent power control.
- Port mapping.
- Equipment reservation.
- Parallel execution.
- Thermal limits.
- Resource locking.
- Firmware-version tracking.
- Centralized logging.
- Automatic health checks.
- Quarantine for failing boards.
- Dashboard and trend analysis.

## Regression principles

- Start with fast smoke tests.
- Run deeper tests after smoke tests pass.
- Separate functional and stress suites.
- Make failures easy to reproduce.
- Track flaky tests.
- Never hide failures through unlimited retries.
- Compare results against historical baselines.
- Preserve logs and measurements.

## Automation terms in more detail

- **Test scheduler:** Chooses which tests run on which boards and when. It should understand board availability, equipment requirements, priority, and resource conflicts.
- **Test runner:** Executes an individual test, enforces timeouts, collects artifacts, and returns a structured result.
- **Board-control layer:** Software that abstracts power cycling, flashing, reset, serial commands, SSH, JTAG, and workload launch.
- **Instrument-control layer:** Software that controls lab equipment such as supplies, oscilloscopes, power monitors, chambers, and analyzers.
- **Artifact:** Any saved evidence from a run, including logs, waveforms, register dumps, firmware images, config files, screenshots, and measurement CSVs.
- **Infrastructure error:** A failure of the test environment rather than the device under test, such as a missing serial adapter, disconnected instrument, host timeout, or corrupt test configuration.
- **Quarantine:** Temporarily removing a board or test station from normal scheduling after repeated failures so it does not pollute regression results.
- **Flaky test:** A test with inconsistent results under apparently identical conditions. Flakiness must be tracked; retries should not hide it.

## What good logs include

A useful log lets another engineer reproduce and interpret a result. Include board serial, SoC identifier, hardware revision, firmware build, host code commit, test parameters, equipment identifiers, voltage/frequency/temperature settings, start and end time, pass/fail reason, recovery actions, and links to artifacts.

## Retry policy

Retries are acceptable for infrastructure recovery, but they should be limited and visible. A device failure that passes after a retry is still valuable data, especially for intermittent bugs. Record every attempt, not only the final result.

---
