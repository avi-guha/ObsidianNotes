# 5. SoC Architecture and Fabric

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/04 Computer Architecture|Previous: 4. Computer Architecture]] | [[Guide Sections/06 Synthetic Workloads|Next: 6. Synthetic Workloads]]

## What is an SoC?

A System on Chip integrates multiple functions onto one silicon die.

A Tesla-like compute SoC may contain:

- CPU cores.
- GPU.
- Neural-network accelerators.
- Camera interfaces.
- Memory controllers.
- DMA engines.
- Security hardware.
- Video-processing blocks.
- Interrupt controllers.
- Timers.
- Debug logic.
- High-speed I/O.
- Power-management logic.
- An internal interconnect or fabric.

## What is the SoC fabric?

The fabric connects SoC components and moves transactions between them.

It handles:

- Address routing.
- Arbitration.
- Data transfer.
- Request prioritization.
- Error responses.
- Backpressure.
- Ordering.
- Coherency.
- Quality of service.

## Initiators and targets

### Initiator

Starts a transaction.

Examples:

- CPU.
- GPU.
- DMA engine.
- NPU.

### Target

Receives a transaction.

Examples:

- DRAM controller.
- Peripheral register block.
- SRAM.
- Camera buffer.

## Fabric concepts

### Bandwidth

Amount of data transferred per unit time.

$\text{Bandwidth} = \frac{\text{Data transferred}}{\text{Time}}$
### Latency

Time between initiating a request and receiving the response.

### Arbitration

Determines which requester gains access when multiple requesters compete.

### Backpressure

A receiver delays or stops incoming traffic because it cannot accept more data.

### Outstanding transactions

Requests that have been issued but have not completed.

### Quality of service

Prioritizes traffic.

For example, camera data may require predictable latency while a background workload can tolerate delays.

### Starvation

A requester waits indefinitely because other requesters are repeatedly selected.

### Deadlock

Two or more components wait on one another and no forward progress occurs.

## How to validate a fabric

Test:

- Every initiator-to-target path.
- Sequential reads and writes.
- Random reads and writes.
- Small and large transactions.
- Bursts.
- Aligned and permitted unaligned accesses.
- Maximum outstanding transactions.
- Simultaneous traffic.
- Priority behavior.
- Backpressure.
- Invalid addresses.
- Timeout handling.
- Error propagation.
- Data integrity.
- Coherency.
- Reset during traffic.

## Example fabric stress strategy

1. CPU performs random memory reads.
2. GPU runs a memory-bandwidth workload.
3. NPU processes tensors.
4. Cameras stream at maximum rate.
5. DMA copies large buffers.
6. Storage performs continuous transfers.
7. Software checks data integrity and performance counters.
8. The test repeats across voltage, frequency, and temperature settings.

## Possible failure symptoms

- Corrupted data.
- Transaction timeout.
- Reduced bandwidth.
- Unfair arbitration.
- Camera frame drops.
- Accelerator stalls.
- Deadlock.
- Incorrect error response.
- Performance collapse during concurrency.
- Intermittent system reset.

## Fabric terms in more detail

- **Transaction:** A read, write, atomic operation, or message moving across the interconnect. Transactions usually contain an address, command, size, attributes, data, and response.
- **Address routing:** The fabric decodes the address and sends the transaction to the correct target, such as DRAM, SRAM, a peripheral register block, or an accelerator.
- **Arbitration:** The choice of which requester gets access when several initiators want the same target or fabric path. Arbitration can be fixed priority, round-robin, weighted, or QoS-aware.
- **Backpressure:** A downstream block signals that it cannot accept more traffic. Correct systems slow down without losing data or deadlocking.
- **Outstanding transaction:** A request that has been issued but not completed. Allowing more outstanding requests can improve throughput but increases ordering and buffering complexity.
- **Ordering:** Rules for which transactions must appear to complete before others. Incorrect ordering can break driver assumptions, DMA descriptors, locks, and device register programming.
- **Coherency:** Hardware support that keeps cached copies consistent across CPUs and coherent agents. Non-coherent agents require explicit software cache maintenance.
- **QoS:** Quality of Service policy that gives latency-sensitive traffic, such as camera frames, enough priority while still allowing bulk traffic, such as GPU memory reads, to make progress.

## Example isolation approach

If camera frames drop only while the GPU is active, do not assume the camera block is broken. First run the camera alone, then the GPU alone, then both together. Record memory bandwidth, fabric error counters, camera FIFO levels, DMA completion timing, interrupt latency, QoS settings, and rail voltage. If drops correlate with full camera buffers and high DRAM bandwidth, the root cause may be fabric arbitration or memory contention rather than the camera receiver.

---
