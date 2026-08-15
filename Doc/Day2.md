* Timer modes: 1.Basic counting mode 2.PWM mode 3.Input Capture Mode 4.Encoder Mode
* Labelling GPIO pins in CUBEMX automatically creates its #define in the code
* HAL has a neat trick — HAL\_GPIO\_<Function>Pin()'s second argument (the pin) can actually take multiple pins OR'd together using the | operator, like GPIO\_PIN\_12 | GPIO\_PIN\_15, since GPIO pins are just bitmasks under the hood. Only possible for pins in the same port.
* Anything between USER CODE BEGIN X / USER CODE END X markers is preserved across regenerations — CubeMX parses those sections separately and merges them back in. Anything you write outside those markers, though, gets wiped and replaced with fresh auto-generated code the next time you hit Generate Code.
* Declare/define before use, always, since C compiles in a single top-to-bottom pass
* When overriding a HAL weak function, copy/verify the exact signature from the HAL header file rather than typing from memory — case mismatches compile fine and fail silently.

