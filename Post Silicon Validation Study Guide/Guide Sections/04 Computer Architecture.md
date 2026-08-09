# 4. Computer Architecture

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/03 Validation Planning and Test Design|Previous: 3. Validation Planning and Test Design]] | [[Guide Sections/05 SoC Architecture and Fabric|Next: 5. SoC Architecture and Fabric]]

## CPU pipeline

A simple pipeline may contain:

1. Instruction fetch.
2. Decode.
3. Execute.
4. Memory access.
5. Writeback.

Pipelining improves throughput by allowing multiple instructions to be processed simultaneously at different stages.

## Pipeline hazards

### Data hazard

An instruction depends on a result that is not ready.

```text
ADD R1, R2, R3
SUB R4, R1, R5
```

The `SUB` depends on the result of the `ADD`.

Solutions include:

- Forwarding.
- Pipeline stalls.
- Out-of-order execution.

### Control hazard

The processor does not know which instruction follows a branch.

Solutions include:

- Branch prediction.
- Speculative execution.
- Pipeline flushing after a misprediction.

### Structural hazard

Multiple operations need the same hardware resource simultaneously.

## Superscalar execution

A superscalar CPU can issue multiple instructions per cycle when dependencies and resources allow it.

## Out-of-order execution

Instructions may execute in a different order from the original program when doing so does not change architectural behavior.

Benefits:

- Hides memory latency.
- Improves use of execution units.
- Increases instruction-level parallelism.

## Interrupts and exceptions

### Interrupt

An asynchronous event caused by external or peripheral hardware.

Examples:

- Timer expiration.
- Incoming network packet.
- Completed DMA transfer.

### Exception

A synchronous event caused by instruction execution.

Examples:

- Divide by zero.
- Invalid instruction.
- Page fault.
- Access violation.

## Memory hierarchy

Typical hierarchy:

```text
Registers
↓
L1 cache
↓
L2 cache
↓
Last-level cache
↓
DRAM
↓
Storage
```

Smaller memories are usually faster and more expensive per bit.

## Cache concepts

### Cache hit

Requested data is found in the cache.

### Cache miss

Requested data is not in the cache and must be fetched from a lower level.

### Temporal locality

Recently accessed data is likely to be accessed again.

### Spatial locality

Data near a recently accessed address is likely to be accessed.

## Cache mapping

### Direct-mapped

Each memory block maps to one cache location.

Advantages:

- Simple.
- Fast.

Disadvantages:

- More conflict misses.

### Set-associative

Each block maps to one set and may occupy several possible ways.

### Fully associative

A block may occupy any cache line.

## Cache write policies

### Write-through

Every cache write also updates lower memory.

### Write-back

Writes update the cache, and modified lines are written to lower memory when evicted.

### Write-allocate

A write miss loads the cache line before writing.

### No-write-allocate

A write miss updates lower memory without loading the line.

## Cache coherence

Cache coherence ensures that multiple processors or agents observe consistent values when they cache the same memory.

Example problem:

1. CPU 0 caches variable `X = 0`.
2. CPU 1 caches `X = 0`.
3. CPU 0 writes `X = 1`.
4. CPU 1 may still hold the old value unless coherence updates or invalidates its copy.

Common conceptual states include:

- Modified.
- Exclusive.
- Shared.
- Invalid.

## Memory ordering

Processors and interconnects may reorder operations for performance.

Memory barriers ensure that specific operations become visible in the required order.

This matters when:

- Communicating with hardware registers.
- Sharing data between CPU cores.
- Starting DMA.
- Updating descriptors before notifying a device.

## Virtual memory

Virtual memory gives each process its own address space.

The system translates:

```text
Virtual address → Physical address
```

### Page table

Stores virtual-to-physical mappings.

### TLB

A Translation Lookaside Buffer caches recent address translations.

### Page fault

Occurs when a virtual page is not mapped or not currently available.

## DMA

Direct Memory Access allows a peripheral or accelerator to transfer data without the CPU copying each word.

Typical flow:

1. CPU prepares a buffer.
2. CPU configures DMA registers.
3. CPU starts the transfer.
4. DMA reads or writes memory.
5. DMA generates an interrupt when complete.

Potential problems:

- Incorrect buffer address.
- Cache-coherency issues.
- Alignment requirements.
- Descriptor corruption.
- Transfer started before data is visible.
- Buffer overwritten too early.

## MMIO

Memory-mapped I/O maps hardware registers into the processor’s address space.

Example:

```c
#define UART_STATUS (*(volatile uint32_t *)0x40001000u)
```

Reading this address performs a hardware-register read rather than an ordinary memory access.

## Architecture interview questions

- What is the difference between a cache and a TLB?
- What causes cache misses?
- What is cache coherence?
- Why are memory barriers needed?
- How does DMA work?
- What happens during an interrupt?
- What makes a workload memory-bound?
- What is branch prediction?
- What is false sharing?
- What is the difference between latency and throughput?

## Validation relevance of architecture concepts

- **Pipeline:** A CPU pipeline overlaps instruction work. Pipeline bugs or stalls can appear as lower-than-expected instruction throughput, branch-heavy workload slowdown, or unexpected performance-counter values.
- **Branch prediction:** The CPU guesses the direction or target of branches. Bad prediction rates can make control-heavy code slow even when arithmetic units are not busy.
- **Speculative execution:** The CPU may execute instructions before knowing whether they are needed. Architecturally incorrect speculative results are discarded, but speculation still affects performance, cache activity, and power.
- **Cache miss:** A request not found in cache. Misses increase latency and memory traffic; they can explain why a workload is memory-bound.
- **TLB miss:** A virtual-to-physical address translation not found in the TLB. TLB misses can hurt workloads with large or irregular memory footprints.
- **False sharing:** Two cores modify different variables that reside in the same cache line. Coherence traffic bounces the line between cores even though the software variables are logically independent.
- **Atomic operation:** A read-modify-write operation that cannot be interrupted or partially observed by other agents. Atomics matter for locks, counters, queues, and shared state.
- **Memory barrier:** An instruction or primitive that constrains ordering. It is often required before ringing a device doorbell after writing DMA descriptors, or before reading data that hardware just produced.
- **MMIO ordering:** Hardware register accesses may require volatile loads/stores plus barriers. Treating MMIO like ordinary memory can cause writes to arrive late, reads to be cached incorrectly, or device configuration to occur out of order.

## Typical hardware/software failure pattern

A common validation issue is cache coherence around DMA. The CPU writes a buffer, but the data remains dirty in cache. The DMA engine reads DRAM before the CPU's cache line is written back, so the device sees stale data. The reverse can also happen: DMA writes new data to DRAM, but the CPU reads an old cached copy. Fixes usually involve coherent DMA mappings, cache clean/invalidate operations, uncached memory, or platform-specific barriers.

---
