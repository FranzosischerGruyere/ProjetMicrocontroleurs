# Agent Guidelines for ProjetMicrocontroleurs

## Build Commands
- **Full build**: `mbed-tools compile -m LPC1768 -t GCC_ARM` (or `mbed compile` for CLI 1)
- **CMake build**: `cmake -S . -B build && cmake --build build`
- **Flash to board**: Add `--flash` flag to compile commands
- **Clean**: `mbed-tools compile -m LPC1768 -t GCC_ARM --clean`

## Code Style Guidelines

### Naming Conventions
- **Functions**: PascalCase (e.g., `ResetLeds()`, `DisplayIntLeds()`)
- **Variables**: camelCase (e.g., `led`, `val`, `temp_sensor`)
- **Constants**: ALL_CAPS with underscores (e.g., `BLINKING_RATE`)
- **Classes/Types**: PascalCase

### Imports and Includes
- Main header: `#include "mbed.h"`
- Peripheral libraries: `#include "LM75B.h"`, `#include "C12832.h"`
- Group includes logically, standard headers first

### Formatting
- Use 4-space indentation
- Opening braces on same line as function/if
- Comments in French for project-specific code
- One statement per line

### Error Handling
- Use basic if/else for error checking
- Return error codes or bool for functions
- No exceptions (embedded constraints)

### Types and Variables
- Use Mbed types: `DigitalOut`, `AnalogIn`, `PwmOut`
- Initialize variables at declaration
- Use `const` for immutable values
- Prefer `int` for integers, `float` for analog values

### Best Practices
- Use `ThisThread::sleep_for()` instead of `wait()`
- Check for LED1 define before using built-in LED
- Use meaningful variable names
- Keep functions focused on single responsibility</content>
<parameter name="filePath">/Users/bison/Mbed Programs/Projet/ProjetMicrocontroleurs_clone/AGENTS.md