# 9. Performance Characterization

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/08 System-Level Power Characterization|Previous: 8. System-Level Power Characterization]] | [[Guide Sections/10 Board and Silicon Bring-Up|Next: 10. Board and Silicon Bring-Up]]

## Key metrics

### Latency

Time required to complete one operation.

### Throughput

Number of operations completed per unit time.

### Bandwidth

Data transferred per unit time.

### Utilization

Percentage of time a resource is active.

### Performance per watt

[
\text{Performance per watt} =
\frac{\text{Performance}}{\text{Power}}
]

## Compute-bound workload

A compute-bound workload is limited by execution resources.

Signs:

- High arithmetic-unit utilization.
- Low memory-stall time.
- Performance increases with clock frequency.
- Memory bandwidth is below its maximum.

## Memory-bound workload

A memory-bound workload is limited by data movement.

Signs:

- High memory bandwidth.
- Many cache misses.
- Long memory stalls.
- Increasing compute frequency provides little improvement.
- Performance improves with better locality.

## Performance-validation method

1. Define the expected theoretical limit.
2. Measure actual throughput.
3. Measure latency.
4. Record utilization.
5. Record memory bandwidth.
6. Record cache behavior.
7. Record power and temperature.
8. Change one workload variable at a time.
9. Compare scaling behavior.
10. Explain the gap between theoretical and observed performance.

## Common causes of poor scaling

- Memory-bandwidth saturation.
- Cache thrashing.
- Synchronization overhead.
- Load imbalance.
- Fabric contention.
- Thermal throttling.
- Power limiting.
- Software serialization.
- Queue or descriptor limits.
- Driver overhead.
- Small workload size.

## Metrics in more detail

- **Latency:** Time for one operation to complete. Low latency matters for interrupts, control loops, frame deadlines, and request/response paths.
- **Throughput:** Completed work per unit time. High throughput matters for bulk compute, memory copies, video pipelines, and accelerator workloads.
- **Bandwidth:** Data movement per unit time, commonly bytes per second. It can refer to DRAM, cache, fabric, PCIe, Ethernet, camera input, or storage.
- **Utilization:** How busy a resource is. High utilization is useful only if the correct resource is busy; 100 percent memory utilization can mean the compute units are starving.
- **Performance per watt:** Efficiency metric. A faster result is not always better if power rises disproportionately.
- **Scaling:** How performance changes as workload size, frequency, number of cores, lanes, queues, or active accelerators changes.

## Explaining the gap to theory

Theoretical peak assumes ideal conditions: no stalls, perfect locality, full vector width, enough parallelism, no throttling, no software overhead, and no contention. Real workloads lose performance to cache misses, synchronization, command submission overhead, non-ideal data shapes, limited queue depth, power limits, and competing traffic.

When comparing expected and measured performance, state which ceiling you are using. Peak arithmetic throughput, memory bandwidth, link bandwidth, and end-to-end application throughput are different ceilings.

---
