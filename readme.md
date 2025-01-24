
# Bare-Metal Programming with libopencm3, with the help of
@lowbyteproductions

## 🛠️ Overview

This project is a hands-on practice in **bare-metal programming**, implementing core functionalities on an STM32 microcontroller using the **libopencm3** library. The focus is on avoiding high-level abstraction layers (such as HAL) to gain a deeper understanding of the hardware and its configuration.

## Key Features
- **Direct Hardware Access**: Programs peripherals directly via registers using libopencm3.
- **Custom Linker Script**: Includes a linker script tailored for the STM32 memory map.
- **Minimal Dependencies**: No HAL or CubeMX-generated code—pure bare-metal programming with libopencm3.
- **Efficient Build System**: Makefile-based setup for streamlined compilation and debugging.

## Highlights in Code

### 1. GPIO Initialization
The GPIO setup in `firmware.c` uses libopencm3 functions to configure the pin for output:

```c
#include <libopencm3/stm32/gpio.h>

void gpio_setup(void) {
    rcc_periph_clock_enable(RCC_GPIOC);
    gpio_mode_setup(GPIOC, GPIO_MODE_OUTPUT, GPIO_PUPD_NONE, GPIO13);
}
```

### 2. Custom Linker Script
The project uses a custom linker script (`linkerscript.ld`) for precise control over memory allocation:

```plaintext
MEMORY
{
    FLASH (rx) : ORIGIN = 0x08000000, LENGTH = 64K
    RAM (rwx)  : ORIGIN = 0x20000000, LENGTH = 20K
}


SECTIONS
{
    .text : { *(.text) } > FLASH
    .bss : { *(.bss) } > RAM
}
```

### 3. Main Firmware Loop
A simple toggle loop demonstrates control over GPIO pins:

```c
#include <libopencm3/stm32/gpio.h>

int main(void) {
    gpio_setup();

    while (1) {
        gpio_toggle(GPIOC, GPIO13);
        for (int i = 0; i < 1000000; i++) { __asm__("nop"); }
    }
}
```

## Repository Structure
```plaintext
├── app/
│   ├── inc/
│   │   └── common-defines.h    # Common macros and definitions
│   ├── src/
│   │   └── firmware.c          # Main firmware logic
│   ├── Makefile                # Build system for the project
│   └── linkerscript.ld         # Custom linker script for STM32
├── .vscode/                    # VSCode debugging and build configuration
└── readme.md                   # Additional documentation
```


