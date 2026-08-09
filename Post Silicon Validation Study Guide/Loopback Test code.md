```c; 
#include <stdint.h>
#include <stdbool.h>
#include <stdio.h>
#include <string.h>

#define UART_TIMEOUT_MS 1000U

/*
 * Replace these functions with your platform's UART driver.
 */
bool uart_init(uint32_t baud_rate);
bool uart_write_byte(uint8_t byte);
bool uart_read_byte(uint8_t *byte, uint32_t timeout_ms);
uint32_t get_time_ms(void);

static bool uart_loopback_test(void)
{
    static const uint8_t test_data[] = {
        0x00, 0x55, 0xAA, 0xFF,
        'U', 'A', 'R', 'T',
        0x12, 0x34, 0x56, 0x78
    };

    uint8_t received[sizeof(test_data)];

    memset(received, 0, sizeof(received));

    /*
     * Send the test pattern.
     */
    for (size_t i = 0; i < sizeof(test_data); ++i) {
        if (!uart_write_byte(test_data[i])) {
            printf("UART write failed at byte %zu\n", i);
            return false;
        }
    }

    /*
     * Read the looped-back data.
     */
    for (size_t i = 0; i < sizeof(received); ++i) {
        if (!uart_read_byte(&received[i], UART_TIMEOUT_MS)) {
            printf("UART receive timeout at byte %zu\n", i);
            return false;
        }
    }

    /*
     * Compare transmitted and received data.
     */
    for (size_t i = 0; i < sizeof(test_data); ++i) {
        if (received[i] != test_data[i]) {
            printf(
                "UART mismatch at byte %zu: sent 0x%02X, received 0x%02X\n",
                i,
                test_data[i],
                received[i]
            );
            return false;
        }
    }

    return true;
}

int main(void)
{
    if (!uart_init(115200U)) {
        printf("UART initialization failed\n");
        return 1;
    }

    if (uart_loopback_test()) {
        printf("UART LOOPBACK TEST PASSED\n");
        return 0;
    }

    printf("UART LOOPBACK TEST FAILED\n");
    return 1;
}
```