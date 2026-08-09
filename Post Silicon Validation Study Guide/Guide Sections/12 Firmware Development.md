# 12. Firmware Development

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/11 Embedded C and C++|Previous: 11. Embedded C and C++]] | [[Guide Sections/13 JTAG, OpenOCD, and Hardware Debugging|Next: 13. JTAG, OpenOCD, and Hardware Debugging]]

## Firmware responsibilities in validation

Firmware may:

- Configure clocks.
- Release resets.
- Initialize memory.
- Program hardware registers.
- Create workloads.
- Read performance counters.
- Capture error registers.
- Control DMA.
- Handle interrupts.
- Communicate with a host test system.
- Support firmware updates.
- Recover from failures.

## State machines

State machines make bring-up and protocol logic easier to reason about.

Example states:

```text
RESET
→ POWER_CHECK
→ CLOCK_CHECK
→ CONFIGURE
→ RUNNING
→ ERROR
→ RECOVERY
```

Each state should define:

- Entry conditions.
- Actions.
- Exit conditions.
- Timeout.
- Error behavior.

## Firmware observability

Good validation firmware exposes:

- Boot-stage markers.
- Error codes.
- Register snapshots.
- Event counters.
- Last successful operation.
- Reset reason.
- Firmware version.
- Hardware revision.
- Timestamps.

## Deterministic behavior

Tests should avoid uncontrolled randomness.

When randomness is used:

- Store the seed.
- Log the seed.
- Make the test reproducible.
- Preserve failing inputs.

## Watchdog timers

A watchdog resets or interrupts a system that stops making progress.

Validation uses:

- Detect hangs.
- Recover boards.
- Record the last known state.
- Prevent one failure from blocking a complete test rack.

## Firmware concepts in more detail

- **Clock configuration:** Programming PLLs, dividers, gates, and muxes so each block receives the correct frequency.
- **Reset control:** Holding blocks in reset until power and clocks are valid, then releasing them in the required order.
- **Memory initialization:** Setting up SRAM, DRAM controllers, training, ECC, address maps, and stack/heap regions before higher-level software uses memory.
- **Register programming:** Writing documented hardware registers to configure modes, interrupts, DMA, clocks, routing, and error handling.
- **DMA control:** Preparing buffers and descriptors, making memory visible to hardware, starting the engine, handling completion, and checking errors.
- **Host communication:** A channel such as UART, USB, Ethernet, JTAG semihosting, or shared memory that lets the test system start workloads and collect results.
- **Watchdog:** A timer that must be periodically serviced by healthy firmware. If firmware hangs, the watchdog can reset the system or trigger diagnostic capture.

## Validation-friendly firmware design

Validation firmware should be boring and observable. It should print boot-stage markers, expose version information, use deterministic seeds, fail with explicit error codes, and preserve the last meaningful state before recovery. Avoid silent retries because they hide intermittent failures and make root cause harder.

---
