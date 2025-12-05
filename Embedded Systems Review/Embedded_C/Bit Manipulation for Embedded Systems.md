
## Concept: Bits are a contract with hardware and protocols 
Bit operations must match datasheet-defined fields exactly; correctness and portability depend on using width, masks, and shifts that are well defined in C. 

### Why does this matter in embedded 
- Register fields require precise masking/shifting without UB
- Protocol packing / unpacking must respect endianness and widths 
- Shifting negative or too far is undefined; rely on fixed-width types. 

### Minimal example: safe field update
``` c++; 
#define REG (*(volatile uint32_t*)0x40000000u)
#define MODE_Pos  4u
#define MODE_Msk  (0x7u << MODE_Pos) // 3-bit field

static inline void reg_set_mode(uint32_t mode) {
  mode &= 0x7u;                // clamp
  uint32_t v = REG;
  v = (v & ~MODE_Msk) | (mode << MODE_Pos);
  REG = v;
}
```

### Implementing get_mode()
``` c++;
static inline uint32_t reg_get_mode(void) {
    return (REG & MODE_Msk) >> MODE_Pos;
}
```

### Takeaways
- Use uint\*\_t never shift signed values. 
- Define \*\_Pos and \*\_Msk macros or enums avoid magic numbers
- Document endianness where on-wire vs in-memory order differs

### Guided Labs 
1. Register field round-trip 
	1. Implement set/get for a 3-bit field and fuzz values 0..7 to very no cross-bit contamination 
``` c++;
#define REG (*(volatile uint32_t*)0x40000000u)
#define MODE_Pos 4u
#define MODE_Msk (0x7u << MODE_Pos) // 3-bit field (bits 6:4)

static inline void reg_set_mode(uint32_t mode) {
    mode &= 0x7u; // clamp to 3 bits
    uint32_t v = REG;
    v = (v & ~MODE_Msk) | (mode << MODE_Pos);
    REG = v;
}

static inline uint32_t reg_get_mode(void) {
    return (REG & MODE_Msk) >> MODE_Pos;
}

void test_reg_field(void) {
    for (uint32_t i = 0; i < 8; i++) {
        reg_set_mode(i);
        uint32_t out = reg_get_mode();
        assert(out == i); // Verify no cross-bit contamination
    }
}

```
1. Protocol pack/unpack 
``` c++;
#include <stdint.h>

typedef struct {
    uint8_t  type;
    uint16_t value;
} msg_t;

static inline uint32_t pack(const msg_t *m) {
    return ((uint32_t)m->type << 24) | ((uint32_t)m->value << 8);
}

static inline void unpack(uint32_t w, msg_t *m) {
    m->type  = (w >> 24) & 0xFF;
    m->value = (w >> 8)  & 0xFFFF;
}

```
Validating: 
``` c++;
void test_pack_unpack(void) {
    msg_t in = { .type = 0xAA, .value = 0x1234 };
    uint32_t w = pack(&in);
    msg_t out;
    unpack(w, &out);
    assert(out.type == in.type && out.value == in.value);
}

```

3. Validate on both little and big endian hosts; discuss network order. 
```c++;
typedef struct { uint8_t type; uint16_t value; } msg_t;
uint32_t pack(const msg_t* m){ return ((uint32_t)m->type<<24)|((uint32_t)m->value<<8); }
void unpack(uint32_t w, msg_t* m){ m->type=(w>>24)&0xFF; m->value=(w>>8)&0xFFFF; }
```
- Big-endian: most significant (MSB) is stored first
	- matches network byte order (used by TCP/IP)
- Little-endian: least significant (LSB) is stored first (x86)

#### Bit manipulation is crucial in embedded systems for: 
- Hardware register access - Setting/clearing individual bits 
- Memory efficiency - Packing multiple values into single variables 
- Performance optimization - Fast bit-level operation 
- Protocol implementation - Parsing binary data formats 
- Flag Management  - Efficient boolean state tracking

#### Key Concepts
- Bitwise operators - AND, OR, XOR, NOT, shift operations 
- Bit fields - Named bit positions in structures 
- Bit masks - Pattern for setting/clearing specific bits 
- Endianness - Byte order considerations
- Bit Counting - Counting set bits efficiently 

## What is Bit Manipulation 
- Bit manipulation is the process of working with individual bits within binary data. 
	- Involves operations that directly manipulate the binary representation of data at the bit level, rather than working with larger data units like bytes or words.
### Core Concepts 
#### Binary Representation: 
- All data in computers is stored in binary (0s and 1s)
- Each bit represents a power of 2. 
- Bit positions are numbered from right to left (0-based)
- Multiple bits combine to represent larger values. 

#### Bit-Level Operations:
- Individual Bit Access: Reading or writing specific bits 
- Bit patterns: Working with groups of bits 
- Bit masks: Using patterns to select specific bits 
- Bit shifting: moving bits left or right

#### Memory Efficiency: 
- Pack multiple boolean values into single bytes 
- Store multiple small values in larger variables 
- Optimize memory usage in constrained systems 
- Reduce data structure sizes

## Binary Number system

#### Positional Notation: 
``` c;
Binary Number: 10110101
Bit Position:  76543210
Value:        128+32+16+4+1 = 181

Bit Position 7: 1 × 2^7 = 128
Bit Position 6: 0 × 2^6 = 0
Bit Position 5: 1 × 2^5 = 32
Bit Position 4: 1 × 2^4 = 16
Bit Position 3: 0 × 2^3 = 0
Bit Position 2: 1 × 2^2 = 4
Bit Position 1: 0 × 2^1 = 0
Bit Position 0: 1 × 2^0 = 1
```

#### Common Bit Patterns:
``` c++;
Power of 2:    1, 2, 4, 8, 16, 32, 64, 128
Binary:        00000001, 00000010, 00000100, 00001000, etc.

All bits set:  11111111 (255 for 8-bit)
All bits clear: 00000000 (0)
```

## Why is Bit Manipulation Important?

### Embedded Systems Requirements 
#### Hardware Interaction: 
- Register Access: Hardware registers often have individual bit controls 
- Flag Management: Status flags and control bits
- Configuration: Device configuration through bit settings 
- Interrupt control: Enable/disable specific interrupt sources

#### Memory Constraints:
- Limited RAM: Pack multiple values into single variables 
- Flash Storage: Optimize storage use 
- Cache Efficiency: Reduce memory footprint
- Bandwidth: Minimize data transfer sizes 

#### Performance Requirements: 
- Real-time systems: Fast bit operations for time-critical code
- Interrupt Handling: Quick flag checking and setting 
- Protocol Processing: Efficient binary data parsing 
- Algorithm Optimization: Fast mathematical operations 

### Real-world Applications 
- Hardware Register Control:
``` c;
// GPIO control
volatile uint32_t* const GPIO_ODR = (uint32_t*)0x40020014;
*GPIO_ODR |= (1 << 5);   // Set bit 5 (turn on LED)
*GPIO_ODR &= ~(1 << 5);  // Clear bit 5 (turn off LED)
```
- Flag Management
``` c;
// Status flags
uint8_t status_flags = 0;
#define FLAG_ERROR     (1 << 0)  // Bit 0
#define FLAG_BUSY      (1 << 1)  // Bit 1
#define FLAG_COMPLETE  (1 << 2)  // Bit 2

// Set flags
status_flags |= FLAG_BUSY;
status_flags |= FLAG_ERROR;

// Check flags
if (status_flags & FLAG_ERROR) {
    // Handle error
}

// Clear flags
status_flags &= ~FLAG_BUSY;
```

- Data Packing: 
``` c;
// Pack multiple values into single variable
uint32_t packed_data = 0;
uint8_t temperature = 25;    // 0-255 range
uint8_t humidity = 60;       // 0-255 range
uint16_t pressure = 1013;    // 0-65535 range

// Pack data
packed_data = (pressure << 16) | (humidity << 8) | temperature;

// Unpack data
temperature = packed_data & 0xFF;
humidity = (packed_data >> 8) & 0xFF;
pressure = (packed_data >> 16) & 0xFFFF;
```

### When Bit Manipulation Matters: 

#### High Impact Scenarios 
- Hardware register access
- Memory-constrained systems 
- Performance-critical applications 
- Binary protocol implementation 
- Flag and status management 

#### Low Impact Scenarios 
- High-level applications with abundant resources 
- Simple applications without hardware interaction 
- Prototype code where clarity is more important 
- Applications with minimal performance requirements 

## Bit Manipulation Concepts 

### Bit-Level Thinking 
#### Individual Bit Operations 
- Set Bit: Make a specific bit 1.
- Clear Bit: Make a specific bit 0.
- Toggle Bit: Invert a specific bit 
- Test Bit: Check a specific bit is set

#### Bit Patterns: 
- Masks: patterns used to select specific bits 
- Flags: Individual bits representing boolean states 
- Fields: Group of bits representing values 
- Signals: Bits representing hardware signals 

#### Bit Position Conventions: 
- LSB(least significant bit): Rightmost bit (position 0)
- MSB (Most significant bit): Leftmost bit 
- Zero-based indexing: Bit positions start at 0
- Power of 2: each position representing 2^n

#### Memory Layout 
##### Byte Organization: 
``` c'
8-bit byte:  [7][6][5][4][3][2][1][0]
16-bit word: [15][14][13][12][11][10][9][8][7][6][5][4][3][2][1][0]
32-bit dword: [31][30]...[2][1][0]
```

#### Endianness considerations
- Little-endian: Least significant byte first 
- Big-endian: Most significant byte first 
- Host byte order: Depends on architecture 

### Performance Characteristics
#### Bit Operation Speed: 
- Single-cycle operations: Most bit operations are very fast 
- Hardware support: Many processors have dedicated bit instructions
- Memory efficiency: Bit operations use minimal memory 
- Cache friendly: small data structures fit in cache 

#### Optimization Opportunities 
- Bit-level parallelism: Process multiple bits simultaneously.
- Lookup tables: Pre-computed bit patterns 
- Specialized instructions: CPU-specific bit operations 
- Compiler optimizations: Automatic bit operation optimization. 

## Bitwise Operators

### What are Bitwise Operators
- Bitwise Operators perform operations on individual bits of their operands. They work at the binary level, comparing or manipulating bits position by position. 
### Operator Categories 
#### Logical Operators 
- AND (&): Result is 1 only if both operands are 1. 
- OR (|): Result is 1 if either operands is 1. 
- XOR(^): Result is 1 if operands are different
- NOT (~): Inverts all bits

#### Shift Operators:
- Left shift (<<): Moves bits left, fills with zeros.
- Right shift (>>) Moves bits right, fills with zeros (logical) or sign bit (arithmetic).

### Basic Operators: 
#### AND (&) Operator 
``` c++;
// Bitwise AND - result bit is 1 only if both operands are 1
uint8_t a = 0b10101010;  // 170
uint8_t b = 0b11001100;  // 204
uint8_t result = a & b;   // 0b10001000 = 136

// Common uses
uint8_t mask_lower_nibble = 0x0F;  // 0b00001111
uint8_t value = 0xAB;              // 0b10101011
uint8_t lower_nibble = value & mask_lower_nibble;  // 0x0B
```
- Applications:
	- Masking: Extracting specific bits 
	- Clearing: Set specific bits to 0
	- Testing: Check if specific bets are set 
	- Range Limiting: Keep values within specific range 

#### OR (|) Operator 
``` c;
// Bitwise OR - result bit is 1 if either operand is 1
uint8_t a = 0b10101010;  // 170
uint8_t b = 0b11001100;  // 204
uint8_t result = a | b;   // 0b11101110 = 238

// Common uses
uint8_t set_bit_3 = 0x08;  // 0b00001000
uint8_t value = 0x01;      // 0b00000001
uint8_t result = value | set_bit_3;  // 0b00001001
```
- Applications 
	- Setting bits: Set specific bits to 1
	- Combining flags: merge multiple flag sets 
	- Default values: Set default bit patterns 
	- Union operations: Combine bit patterns 

#### XOR(^) Operator:
``` c;
// Bitwise XOR - result bit is 1 if operands are different
uint8_t a = 0b10101010;  // 170
uint8_t b = 0b11001100;  // 204
uint8_t result = a ^ b;   // 0b01100110 = 102

// Common uses
uint8_t toggle_bit_2 = 0x04;  // 0b00000100
uint8_t value = 0x01;         // 0b00000001
uint8_t result = value ^ toggle_bit_2;  // 0b00000101
```
Applications: 
- Toggling bits: Invert specific bits 
- Encryption: Simple bit-level encryption 
- Parity checking: Error detection 
- Finding differences: Identify changed bits 

#### Not (~) Operator:
```c;
// Bitwise NOT - inverts all bits
uint8_t a = 0b10101010;  // 170
uint8_t result = ~a;      // 0b01010101 = 85

// Common uses
uint8_t mask = 0x0F;      // 0b00001111
uint8_t inverted_mask = ~mask;  // 0b11110000
```
Applications: 
- Inverting masks: Creating complementary bit patterns
- Clearing bits: Use with AND to clear specific bits 
- Bit counting: Count unset bits
- Range Operations: Create exclusion masks 

### Shift Operators 
#### Left Shift (<<)
``` c;
// Left shift - moves bits left, fills with zeros
uint8_t value = 0b00000001;  // 1
uint8_t result = value << 3;  // 0b00001000 = 8

// Common uses
uint8_t create_mask = 1 << 5;  // 0b00100000 (bit 5)
uint32_t multiply_by_8 = value << 3;  // Multiply by 8
```
Applications: 
- Creating masks: Generate bit patterns 
- Multiplication: Multiply by powers of 2
- Bit positioning: Place bits in specific positions 
- Flag creation: create flag constants

#### Right Shift (>>)
``` c;
// Right shift - moves bits right, fills with zeros
uint8_t value = 0b10001000;  // 136
uint8_t result = value >> 3;  // 0b00010001 = 17

// Common uses
uint8_t extract_upper_nibble = value >> 4;  // Get upper 4 bits
uint32_t divide_by_8 = value >> 3;  // Divide by 8
```
Applications:
- Extracting fields: Get specific bit ranges
- Division: Divide by powers of 2
- Bit counting: Count trailing 0s. 
- Range extraction: Extract specific bit positions 

## Bit Fields 
### What are Bit fields 
- Bit fields are named bit positions within structures that allow you to access individual bits or groups of bits by name rather than position 

#### Bit Field Concepts 
Named Access 
- Individual bits or bit groups that have names 
- Access bits using field names instead of masks 
- Compuler handles bit positioning automatically 
- Provides type safety and readability 

Memory Efficiency: 
- Pack multiple small values into single variables 
- Reduce memory usage in constrained systems 
- Optimize data structure sizes 
- Maintain readability while saving space 

Hardware Mapping: 
- Map directly to hardware register layouts 
- Match hardware bit fields definitions 
- Ensure correct bit positioning 
- Maintain hardware compatibility 

#### Bit Field implementation
Basic Bit Fields:
``` c;
// Simple bit field structure
typedef struct {
    uint8_t error : 1;      // 1 bit for error flag
    uint8_t busy : 1;       // 1 bit for busy flag
    uint8_t complete : 1;   // 1 bit for complete flag
    uint8_t reserved : 5;   // 5 bits reserved
} status_flags_t;

// Usage
status_flags_t flags = {0};
flags.error = 1;        // Set error flag
flags.busy = 1;         // Set busy flag

if (flags.complete) {   // Check complete flag
    // Handle completion
}
```

Hardware Register Mapping: 
``` c; 
// GPIO configuration register
typedef struct {
    uint32_t mode : 2;      // GPIO mode (00=Input, 01=Output, 10=Alternate, 11=Analog)
    uint32_t pull_up : 1;   // Pull-up resistor
    uint32_t pull_down : 1; // Pull-down resistor
    uint32_t speed : 2;     // Output speed
    uint32_t reserved : 26; // Reserved bits
} gpio_config_t;

// Usage
gpio_config_t config = {0};
config.mode = 1;        // Output mode
config.pull_up = 1;     // Enable pull-up
config.speed = 2;       // High speed
```

Data Packing 
``` c;
// Pack multiple values into single variable
typedef struct {
    uint32_t temperature : 8;   // 8 bits for temperature (0-255)
    uint32_t humidity : 8;      // 8 bits for humidity (0-255)
    uint32_t pressure : 16;     // 16 bits for pressure (0-65535)
} sensor_data_t;

// Usage
sensor_data_t data = {0};
data.temperature = 25;   // Set temperature
data.humidity = 60;      // Set humidity
data.pressure = 1013;    // Set pressure
```

### Bit Masks 

#### What are Bit Masks 
- Bit masks are patterns of bits used to select, modify, or test specific bits within a larger value. They provide a systematic way to work with individual bits or groups of bits. 


#### Mask Concepts
Selection Patterns: 
- Single bit masks: Select individual bits 
- Multi-bit masks: Select groups of bits 
- Range Masks: Select bit ranges 
- Inverted masks: Select everything except specific bits 

Mask Operations: 
- & with mask: Extract specific bits 
- | with mask: Set specific bits 
- XOR with mask: Toggle specific bits 
- NOT with mask: Invert selection 

#### Mask Implementation 

Single Bit Masks: 
``` c;
// Define single bit masks
#define BIT_0  (1 << 0)   // 0b00000001
#define BIT_1  (1 << 1)   // 0b00000010
#define BIT_2  (1 << 2)   // 0b00000100
#define BIT_3  (1 << 3)   // 0b00001000
#define BIT_4  (1 << 4)   // 0b00010000
#define BIT_5  (1 << 5)   // 0b00100000
#define BIT_6  (1 << 6)   // 0b01000000
#define BIT_7  (1 << 7)   // 0b10000000

// Usage
uint8_t value = 0x42;    // 0b01000010
uint8_t bit_1 = value & BIT_1;  // Check if bit 1 is set
value |= BIT_3;          // Set bit 3
value &= ~BIT_5;         // Clear bit 5
value ^= BIT_2;          // Toggle bit 2
```

Multi Bit Masks: 
``` c; 
// Define multi-bit masks
#define NIBBLE_LOWER  0x0F    // 0b00001111 (lower 4 bits)
#define NIBBLE_UPPER  0xF0    // 0b11110000 (upper 4 bits)
#define BYTE_LOWER    0xFF    // 0b11111111 (lower 8 bits)
#define BYTE_UPPER    0xFF00  // 0b1111111100000000 (upper 8 bits)

// Usage
uint16_t value = 0x1234;
uint8_t lower_byte = value & BYTE_LOWER;   // 0x34
uint8_t upper_byte = (value & BYTE_UPPER) >> 8;  // 0x12
```

Range Masks 
``` c; 
// Create masks for specific bit ranges
#define MASK_3_BITS(n)    ((1 << n) - 1)  // n consecutive bits starting from 0
#define MASK_RANGE(start, end) (((1 << (end - start + 1)) - 1) << start)

// Usage
#define MASK_BITS_2_4  MASK_RANGE(2, 4)   // 0b00011100
#define MASK_BITS_0_2  MASK_3_BITS(3)     // 0b00000111

uint8_t value = 0x5A;    // 0b01011010
uint8_t bits_2_4 = (value & MASK_BITS_2_4) >> 2;  // Extract bits 2-4
```


## Bit shifting
- Bit shifting moves bits left or right within a value, effectively multiplying or dividing by powers of two. It is fundamental operation for bit manipulation and arithmetic optimization. 

### Shift Concepts 

#### Direction 
- Left Shift (<<): Moves bits left, multiplies by 2^n
- Right shift (>>): Moves bits right, divides by 2^n
- Zero fill: Shifts fill with zeros (logical shift)
- Sign fill: Right shifts may fill with a sign bit 

#### Applications: 
- Multiplication/Division: Fast arithmetic operations 
- Bit positioning: Place bits in specific positions 
- Field extraction: Extract specific bit ranges 
- Mask Creation: Generate bit patterns 

#### Shift Implementation 
Arithmetic Operations: 
``` c;
// Fast multiplication and division by powers of 2
uint32_t value = 10;
uint32_t multiplied = value << 2;   // 10 * 4 = 40
uint32_t divided = value >> 1;      // 10 / 2 = 5

// Power of 2 calculations
uint32_t power_of_2 = 1 << n;       // 2^n
uint32_t mask_for_n_bits = (1 << n) - 1;  // n consecutive 1s
```

Field Operations: 
``` c;
// Fast multiplication and division by powers of 2
uint32_t value = 10;
uint32_t multiplied = value << 2;   // 10 * 4 = 40
uint32_t divided = value >> 1;      // 10 / 2 = 5

// Power of 2 calculations
uint32_t power_of_2 = 1 << n;       // 2^n
uint32_t mask_for_n_bits = (1 << n) - 1;  // n consecutive 1s
```

Bit Positioning Operations: 
``` c; 
// Set bit at position n
uint32_t set_bit(uint32_t value, uint8_t position) {
    return value | (1 << position);
}

// Clear bit at position n
uint32_t clear_bit(uint32_t value, uint8_t position) {
    return value & ~(1 << position);
}

// Toggle bit at position n
uint32_t toggle_bit(uint32_t value, uint8_t position) {
    return value ^ (1 << position);
}

// Test bit at position n
bool test_bit(uint32_t value, uint8_t position) {
    return (value & (1 << position)) != 0;
}
```

## Hardware Register Operations 
- Involve manipulating individual bits in hardware registers to control device behavior, read status information, or configure hardware peripherals 
#### Register Operation Concepts 
Memory-Mapped Registers: 
- Hardware registers appear as memory addresses 
- Individual bits control specific hardware functions 
- Reading/writing registers affects hardware behavior 
- Some registers are read-only or write only 

Bit-level Control: 
- Control bits: Configure hardware behavior
- Status bits: Read hardware state 
- Interrupt bits: Control interrupt behavior 
- Configuration bits: Set device parameters: 

#### Hardware Register Implementation 

GPIO Control : 
``` c;
// GPIO register definitions
volatile uint32_t* const GPIOA_ODR = (uint32_t*)0x40020014;
volatile uint32_t* const GPIOA_IDR = (uint32_t*)0x40020010;
volatile uint32_t* const GPIOA_CRL = (uint32_t*)0x40020000;

// GPIO control functions
void gpio_set_pin(uint8_t pin) {
    *GPIOA_ODR |= (1 << pin);
}

void gpio_clear_pin(uint8_t pin) {
    *GPIOA_ODR &= ~(1 << pin);
}

void gpio_toggle_pin(uint8_t pin) {
    *GPIOA_ODR ^= (1 << pin);
}

bool gpio_read_pin(uint8_t pin) {
    return (*GPIOA_IDR & (1 << pin)) != 0;
}
```

UART Configuration: 
``` c; 
// UART register definitions
volatile uint32_t* const UART_CR1 = (uint32_t*)0x40011000;
volatile uint32_t* const UART_CR2 = (uint32_t*)0x40011004;
volatile uint32_t* const UART_SR = (uint32_t*)0x40011000;

// UART control bits
#define UART_ENABLE      (1 << 13)
#define UART_TX_ENABLE   (1 << 3)
#define UART_RX_ENABLE   (1 << 2)
#define UART_TX_READY    (1 << 7)
#define UART_RX_READY    (1 << 5)

// UART configuration
void uart_enable(void) {
    *UART_CR1 |= UART_ENABLE | UART_TX_ENABLE | UART_RX_ENABLE;
}

void uart_disable(void) {
    *UART_CR1 &= ~(UART_ENABLE | UART_TX_ENABLE | UART_RX_ENABLE);
}

bool uart_tx_ready(void) {
    return (*UART_SR & UART_TX_READY) != 0;
}

bool uart_rx_ready(void) {
    return (*UART_SR & UART_RX_READY) != 0;
}
```

### Common Bit Tricks 
Bit tricks are clever techniques that use bit manipulation to solve problems efficiently. Often provide faster or more memory-efficient solution that traditional approaches. 

1. Counting Set bits 
``` c; 
// Count number of 1 bits in a value
uint32_t count_set_bits(uint32_t value) {
    uint32_t count = 0;
    while (value) {
        count += value & 1;
        value >>= 1;
    }
    return count;
}

// Fast method using lookup table
uint32_t count_set_bits_fast(uint32_t value) {
    static const uint8_t lookup[256] = {
        0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4,
        // ... (complete lookup table)
    };
    
    return lookup[value & 0xFF] + 
           lookup[(value >> 8) & 0xFF] + 
           lookup[(value >> 16) & 0xFF] + 
           lookup[(value >> 24) & 0xFF];
}
```
Finding Lowest Set Bit: 
``` c;
// Find position of lowest set bit
int find_lowest_set_bit(uint32_t value) {
    if (value == 0) return -1;
    return __builtin_ctz(value);  // Count trailing zeros
}

// Alternative method
int find_lowest_set_bit_alt(uint32_t value) {
    if (value == 0) return -1;
    return value & -value;  // Isolates lowest set bit
}
```

Power of 2 Check 
``` c;
// Check if value is power of 2
bool is_power_of_2(uint32_t value) {
    return value != 0 && (value & (value - 1)) == 0;
}

// Get next power of 2
uint32_t next_power_of_2(uint32_t value) {
    value--;
    value |= value >> 1;
    value |= value >> 2;
    value |= value >> 4;
    value |= value >> 8;
    value |= value >> 16;
    return value + 1;
}
```

Bit Reversal
``` c; 
// Reverse bits in a byte
uint8_t reverse_bits(uint8_t value) {
    value = ((value & 0x55) << 1) | ((value & 0xAA) >> 1);
    value = ((value & 0x33) << 2) | ((value & 0xCC) >> 2);
    value = ((value & 0x0F) << 4) | ((value & 0xF0) >> 4);
    return value;
}
```

## Performance Considerations 
### What affects bit operation performance
- Bit operation performance depends on several factors including hardware support, compiler optimizations, and operation complexity. 

#### Performance Factors
Hardware Support: 
- Dedicated instructions: Some processors have specialized bit instructions 
- Instruction latency: Different operations have different timing 
- Pipeline efficiency: How well operations fit into CPU pipeline 
- Cache Behavior: Memory access patterns affect performance 

Compiler Optimizations: 
- Constant folding: Compiler may pre-compute constant expressions 
- Instruction selection: Compiler chooses optimal machine instructions 
- Loop optimization Compiler may optimize bit operation loops 
- Inlining: Small bit functions may be inlined. 

Operation Complexity: 
- Single operations: Individual bit operations are very fast 
- Complex patterns: Multiple operations may be slower 
- Memory access: Bit operations on memory are slower than registers 
- Branch prediction: Conditional bit operations may be slower 


#### Performance Optimization 
Use Built-in functions 
``` c; 
// Use compiler built-ins when available
uint32_t count_bits = __builtin_popcount(value);
uint32_t leading_zeros = __builtin_clz(value);
uint32_t trailing_zeros = __builtin_ctz(value);
uint32_t bit_reverse = __builtin_bitreverse32(value);
```

Optimize Common Patterns 
``` c;
// Optimize bit field operations
#define SET_BIT(reg, bit)    ((reg) |= (1 << (bit)))
#define CLEAR_BIT(reg, bit)  ((reg) &= ~(1 << (bit)))
#define TOGGLE_BIT(reg, bit) ((reg) ^= (1 << (bit)))
#define READ_BIT(reg, bit)   (((reg) >> (bit)) & 1)

// Use lookup tables for complex operations
static const uint8_t bit_count_table[256] = {
    // Pre-computed bit counts for all byte values
};
```

Memory Access Optimization 
``` c; 
// Minimize memory accesses
uint32_t read_modify_write(uint32_t* reg, uint32_t mask, uint32_t value) {
    uint32_t current = *reg;
    current = (current & ~mask) | (value & mask);
    *reg = current;
    return current;
}
```

## Implementation 
#### Complete Bit Manipulation Example 
``` c;
#include <stdint.h>
#include <stdbool.h>

// Hardware register definitions
#define GPIOA_BASE    0x40020000
#define GPIOA_ODR     (GPIOA_BASE + 0x14)
#define GPIOA_IDR     (GPIOA_BASE + 0x10)
#define GPIOA_CRL     (GPIOA_BASE + 0x00)

// GPIO register pointers
volatile uint32_t* const gpio_odr = (uint32_t*)GPIOA_ODR;
volatile uint32_t* const gpio_idr = (uint32_t*)GPIOA_IDR;
volatile uint32_t* const gpio_crl = (uint32_t*)GPIOA_CRL;

// Bit manipulation macros
#define SET_BIT(reg, bit)    ((reg) |= (1 << (bit)))
#define CLEAR_BIT(reg, bit)  ((reg) &= ~(1 << (bit)))
#define TOGGLE_BIT(reg, bit) ((reg) ^= (1 << (bit)))
#define READ_BIT(reg, bit)   (((reg) >> (bit)) & 1)

// Status flags using bit fields
typedef struct {
    uint8_t error : 1;
    uint8_t busy : 1;
    uint8_t complete : 1;
    uint8_t timeout : 1;
    uint8_t reserved : 4;
} status_flags_t;

// GPIO control functions
void gpio_set_pin(uint8_t pin) {
    SET_BIT(*gpio_odr, pin);
}

void gpio_clear_pin(uint8_t pin) {
    CLEAR_BIT(*gpio_odr, pin);
}

void gpio_toggle_pin(uint8_t pin) {
    TOGGLE_BIT(*gpio_odr, pin);
}

bool gpio_read_pin(uint8_t pin) {
    return READ_BIT(*gpio_idr, pin);
}

// Bit counting functions
uint32_t count_set_bits(uint32_t value) {
    uint32_t count = 0;
    while (value) {
        count += value & 1;
        value >>= 1;
    }
    return count;
}

bool is_power_of_2(uint32_t value) {
    return value != 0 && (value & (value - 1)) == 0;
}

// Main function
int main(void) {
    status_flags_t flags = {0};
    
    // Set status flags
    flags.error = 1;
    flags.busy = 1;
    
    // GPIO operations
    gpio_set_pin(5);      // Turn on LED
    gpio_clear_pin(6);    // Turn off LED
    
    // Check GPIO state
    if (gpio_read_pin(0)) {
        // Button pressed
        gpio_toggle_pin(5);  // Toggle LED
    }
    
    // Bit counting example
    uint32_t test_value = 0x12345678;
    uint32_t bit_count = count_set_bits(test_value);
    
    // Power of 2 check
    if (is_power_of_2(64)) {
        // 64 is a power of 2
    }
    
    return 0;
}
```

## Common Pitfalls 
#### Sign Extension Issues 
Problem: Right shifting signed values can cause sign extension. 
Solution: Use unsigned types for bit operations 

### Undefined Behavior with Shifts 
Problem: Shifting by negative amounts or beyond bit width.
Solution: Validate shift amounts

### Bit Field Portability 
Problem: Bit field behavior varies between compilers 
Solution: Use explicit bit manipulation instead 

### Endianness Issues 
Problem: Bit operations may behave differently on different architectures 
Solution: Use portable bit manipulation 

## Best Practices

### Use Appropriate Types
- Unsigned types: Use for bit operations to avoid sign issues 
- Explicit sizes: Use uint8_t, uint16_t, uint16_t for clarity. 
- Avoid char: char might be signed or unsigned depending on the platform 
- Consider alignment: Use appropriate types for hardware registers. 

### Validate Operations 
- Check shift amounts: Ensure shifts are within valid range 
- Validate bit positions: Check that bit positions are valid 
- Test edge cases: Test with 0, max values, and boundary conditions 
- Use assertions: Add runtime checks in debug builds 

### Document Bit Layouts 
- Clear documentation: Document bit field layouts and meanings 
- Use constants: Define bit positions and masks as constants 
- Comment complex operations: Explain the non-obvious bit manipulations 
- Maintain consistency: Use consistent naming conventions 

### Optimize for Performance 
- Use built ins: Use compiler built-in functions when available 
- Minimize memory access: Cache register values when possible 
- Use lookup tables: For complex operations that are called frequently 
- Profile critical code: Measure performance of bit operations in critical paths. 

## Ensure Portability 
- Avoid undefined behavior: Don't rely on undefined bit operations 
- Use Standard operation: Stick to well-defined bit operations 
- Test on multiple platforms: Verify behavior across different architectures 
- Consider endianness: Handle byte order issues appropriately 


## Interview Questions 
### Basic: 
- **Basic Operators**
	- And, Or, XOR, NOT 
	- Left Shift Right Shift
	- Each operators has specific use cases / Behaviour 
- **How do we set, clear, and toggle a specific bit **
	- Set: Value |= (1 << bit_position)
	- Clear: value &= ~(1 << bit_Position)
	- Toggle: value ^= (1 << bit position)
- **What is the difference between logical and arithmetic shift?**
    - Logical shift: Always fills with zeros
    - Arithmetic shift: Right shift may fill with sign bit
    - C standard doesn't specify behavior for negative values
### Advanced 
- **How do we count the number of bits in a value**
	-  Loop method: Check each bit individually
	- Lookup table: Pre-computed bit counts
	- Built-in functions: __builtin_popcount()
	- Brian Kernighan's algorithm: value &= (value - 1)
- **How can we check if a number is a power of two **
	- Use: (value != 0) && ((value & (value - 1)) == 0)
	- Power of 2 has exactly one set bit
	- Subtracting 1 clears the lowest set bit
- **How can you reverse bits in a value:**
	- Byte reversal: Lookup table or bit manipulation 
	- Word reversal: Built in functions or divide and conquer 
	- Keep in mind endianness and data size. 
	 ``` c;
	 // Reverse bits in a byte
	uint8_t reverse_bits(uint8_t value) {
    value = ((value & 0x55) << 1) | ((value & 0xAA) >> 1);
    value = ((value & 0x33) << 2) | ((value & 0xCC) >> 2);
    value = ((value & 0x0F) << 4) | ((value & 0xF0) >> 4);
    return value;
}
	  ```
**Implementation Questions**
- Write a function to find lowest set bit 
```
return n & -n 
```
- Implement a function to check if two numbers have opposite signs 
``` c;
return (x^y) < 0
```
- Write code to swap two variables without using a temporary variable
``` c;
void swap(int *a, int *b) {

    *a = *a ^ *b;

    *b = *a ^ *b;

    *a = *a ^ *b;

}
```

- Design a function to round up to next power of 2. 
``` c;
int roundUpToNextPowerOf2(int n) {

    if (n <= 0) return 1;

    int power = 1;

    while (power < n) power <<= 1;

    return power;

}
```