# 6. Synthetic Workloads

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/05 SoC Architecture and Fabric|Previous: 5. SoC Architecture and Fabric]] | [[Guide Sections/07 High-Speed SerDes Validation|Next: 7. High-Speed SerDes Validation]]

## Purpose

A synthetic workload is designed to produce controlled and repeatable stress on a hardware block.

It should isolate:

- Compute pressure.
- Memory pressure.
- Control-flow pressure.
- Data-path pressure.
- Interface bandwidth.
- Buffer limits.
- Power consumption.
- Thermal load.
- Concurrency interactions.

## Workload dimensions

Vary:

- Input size.
- Data pattern.
- Precision.
- Read/write ratio.
- Access pattern.
- Thread count.
- Queue depth.
- Duration.
- Clock frequency.
- Concurrency.
- Memory location.
- Alignment.

## CPU workloads

### Compute-heavy

- Integer arithmetic.
- Floating-point operations.
- Vector instructions.
- Cryptographic operations.

### Memory-heavy

- Sequential streaming.
- Random access.
- Pointer chasing.
- Cache-thrashing patterns.

### Branch-heavy

- Unpredictable branches.
- Large decision trees.
- Irregular control flow.

## GPU workloads

- Matrix multiplication.
- Vector addition.
- Compute-heavy shaders or kernels.
- Memory-bandwidth tests.
- Random memory access.
- Cache-thrashing patterns.
- Maximum occupancy.
- Rapid kernel launch and completion.
- Context switching.
- Concurrent copy and computation.

### GPU validation questions

- Is the result correct?
- Is execution deterministic?
- Does performance scale with workload size?
- Does the workload reach expected utilization?
- Are errors correlated with temperature or voltage?
- Does the GPU interfere with cameras or the NPU?

## Neural-network accelerator workloads

- Convolution.
- Matrix multiplication.
- Activation functions.
- Tensor reshaping.
- Different tensor dimensions.
- Different batch sizes.
- INT8, FP16, or supported formats.
- Maximum on-chip memory use.
- Maximum DRAM bandwidth.
- Misaligned or unusual dimensions.
- Repeated execution for determinism.

### Validation method

1. Generate known input data.
2. Compute a trusted reference result.
3. Run the accelerator.
4. Compare output with an allowed tolerance.
5. Record execution time, power, utilization, and errors.
6. Repeat under system stress.

## Camera interface workloads

Test:

- Every supported camera port.
- Minimum and maximum resolution.
- Minimum and maximum frame rate.
- Multiple cameras.
- Long-duration streaming.
- Rapid start and stop.
- Disconnect and reconnect.
- Frame synchronization.
- Invalid packets.
- Buffer overflow.
- Backpressure.
- DMA errors.
- Memory congestion.
- Reset during capture.

Monitor:

- Dropped frames.
- Corrupt frames.
- CRC or protocol errors.
- Synchronization loss.
- Buffer occupancy.
- Interrupt counts.
- DMA completion.
- Timestamp consistency.

## Useful data patterns

Data patterns can reveal data-dependent hardware failures.

Examples:

- All zeros.
- All ones.
- Alternating `1010`.
- Alternating `0101`.
- Walking one.
- Walking zero.
- Incrementing sequence.
- Pseudorandom data.
- Repeated fixed patterns.

## What each workload dimension means

- **Input size:** The amount of data processed. Small inputs may fit in cache; large inputs may expose DRAM, fabric, or TLB limits.
- **Data pattern:** The bit pattern or value distribution. Some bugs appear only with all-zeros, all-ones, walking-bit, denormal floating-point, saturated integer, or pseudorandom data.
- **Precision:** Numeric format such as INT8, FP16, FP32, or fixed point. Different precision modes may use different hardware paths.
- **Read/write ratio:** The balance between loading data and storing results. This changes cache behavior, memory bandwidth, and power.
- **Access pattern:** Sequential, random, strided, pointer-chasing, or tiled access. It determines cache locality and memory-controller efficiency.
- **Thread count:** Number of concurrent software workers. It can expose synchronization overhead, cache contention, interrupt pressure, and scheduler issues.
- **Queue depth:** Number of pending operations. Higher depth may improve utilization but can expose descriptor, timeout, ordering, or backpressure bugs.
- **Alignment:** Whether buffers start on natural boundaries. Misalignment can expose hardware restrictions, extra memory transactions, or driver assumptions.

## Reference-result strategy

Synthetic workloads should have a trusted way to decide correctness. For CPU and accelerator workloads, that often means running a simple reference implementation on the host or CPU, then comparing device output with exact matching for integer data or tolerance-based matching for floating-point data.

For long stress tests, do not wait until the end to check correctness. Add periodic signatures, checksums, sequence numbers, or frame counters so the log shows when corruption first appeared.

## Making workloads useful for debug

A good workload has knobs. You should be able to reduce active blocks, data size, rate, queue depth, thread count, and duration. If the only available test is "run everything for two hours," it is hard to isolate the cause. A useful stress test can be scaled down to a small failing case and scaled up again for regression.

---
