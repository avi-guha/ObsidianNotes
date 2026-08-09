# 11. Embedded C and C++

[[Post Silicon Validation Study Guide|Back to study guide index]] | [[Guide Sections/10 Board and Silicon Bring-Up|Previous: 10. Board and Silicon Bring-Up]] | [[Guide Sections/12 Firmware Development|Next: 12. Firmware Development]]

## `volatile`

`volatile` tells the compiler that a value may change outside normal program flow.

Common uses:

- Hardware registers.
- Variables modified by interrupts.
- Variables modified by another execution context.

```c
volatile uint32_t status;
```

Without `volatile`, the compiler may optimize away repeated reads.

> [!warning]
> `volatile` does not make an operation atomic and does not replace locks or memory barriers.

## Memory-mapped register example

```c
#include <stdint.h>

#define CTRL_REG_ADDR 0x40001000u
#define CTRL_REG (*(volatile uint32_t *)CTRL_REG_ADDR)

#define ENABLE_BIT (1u << 3)

void enable_block(void)
{
    CTRL_REG |= ENABLE_BIT;
}
```

## Read-modify-write risk

This code:

```c
CTRL_REG |= ENABLE_BIT;
```

performs:

1. Read register.
2. Modify value.
3. Write register.

It may be unsafe when:

- The register contains write-one-to-clear bits.
- Another context changes the register.
- Reading the register has side effects.
- Hardware updates bits between the read and write.

Some devices provide dedicated:

- Set registers.
- Clear registers.
- Toggle registers.

## Bit operations

### Set a bit

```c
value |= (1u << bit);
```

### Clear a bit

```c
value &= ~(1u << bit);
```

### Toggle a bit

```c
value ^= (1u << bit);
```

### Test a bit

```c
bool is_set = (value & (1u << bit)) != 0u;
```

### Extract a field

```c
uint32_t field = (value >> shift) & mask;
```

## Pointer concepts

Know:

- Pointer declaration.
- Dereferencing.
- Pointer arithmetic.
- Null pointers.
- Dangling pointers.
- Array decay.
- Function pointers.
- `const` correctness.

## Stack versus heap

### Stack

- Automatic local variables.
- Fast allocation.
- Limited capacity.
- Lifetime tied to function scope.

### Heap

- Dynamic allocation.
- Must be managed.
- Can fragment.
- Often avoided or tightly controlled in embedded systems.

## Struct padding

The compiler may insert padding to satisfy alignment.

```c
struct Example {
    uint8_t a;
    uint32_t b;
};
```

The structure may occupy more than five bytes.

This matters for:

- Hardware descriptors.
- Network packets.
- File formats.
- Register maps.

## Endianness

### Little-endian

Least significant byte is stored at the lowest address.

### Big-endian

Most significant byte is stored at the lowest address.

## Interrupt service routines

An ISR should generally:

- Be short.
- Avoid blocking.
- Avoid heavy computation.
- Record necessary state.
- Clear the interrupt properly.
- Signal the main loop or a worker task.

## Polling versus interrupts

### Polling

Advantages:

- Simple.
- Predictable.

Disadvantages:

- Wastes CPU time.
- May miss timing requirements if polling is too slow.

### Interrupts

Advantages:

- Efficient for infrequent events.
- Faster response.

Disadvantages:

- More complex.
- Race conditions.
- Priority interactions.

## Race conditions

A race condition occurs when behavior depends on the timing of concurrent operations.

Example:

```c
volatile uint32_t count;

void increment(void)
{
    count++;
}
```

`count++` may involve a read, increment, and write. An interrupt between these steps can cause an update to be lost.

## Timeouts

Never allow validation software to wait forever.

```c
#include <stdbool.h>
#include <stdint.h>

bool wait_for_ready(uint32_t timeout_cycles)
{
    while (timeout_cycles > 0u) {
        if ((STATUS_REG & READY_MASK) != 0u) {
            return true;
        }

        timeout_cycles--;
    }

    return false;
}
```

Benefits:

- Prevents regression hangs.
- Produces a clear failure.
- Allows automatic recovery.
- Makes large-scale testing practical.

## Error-handling principles

- Check every return value.
- Preserve the original error.
- Include enough context in logs.
- Distinguish timeout, communication, and data errors.
- Avoid silent retries.
- Limit retries.
- Reset only when justified.
- Report recovery actions.

## Embedded terms in more detail

- **`volatile`:** Prevents the compiler from removing or merging accesses to an object. It is necessary for MMIO registers and interrupt-updated variables, but it does not create atomicity, ordering, or mutual exclusion.
- **MMIO register:** A hardware register accessed through a memory address. Reading or writing that address can trigger hardware behavior, so normal compiler optimizations must be constrained.
- **Read-modify-write:** A sequence that reads a value, changes bits in software, then writes it back. It can be unsafe for registers with write-one-to-clear bits, side-effect reads, or hardware-updated status bits.
- **Write-one-to-clear:** A status-bit convention where writing `1` clears the bit and writing `0` leaves it unchanged. A careless read-modify-write can accidentally clear unrelated events.
- **ISR:** Interrupt Service Routine. It runs in response to an interrupt and should do the minimum required work before returning or waking a lower-priority task.
- **Atomicity:** An operation is indivisible from the perspective of other execution contexts. `count++` is usually not atomic because it can compile into load, add, and store.
- **Critical section:** Code that must not be interrupted or entered concurrently because it manipulates shared state.
- **Alignment:** Address placement required or preferred by the CPU or device. Misaligned accesses may be slower, fault, or be unsupported by DMA hardware.
- **Strict aliasing:** C/C++ compiler rules about accessing objects through compatible pointer types. Violating these rules can produce surprising optimized code.

## Register-access habits

Prefer named masks, shifts, and helper functions over magic numbers. For hardware registers, always check the register description for reset value, read/write permissions, side effects, reserved bits, and whether writes must preserve existing values. Reserved bits should usually be written with documented reset values or preserved as specified.

For polling loops, include a timeout and return a useful error. A validation test that hangs forever loses the most important information: which operation stopped making progress.

---
