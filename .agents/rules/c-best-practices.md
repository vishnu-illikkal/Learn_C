# C Programming Standards: Best Practices & Memory Safety Rules

When generating, teaching, refactoring, or reviewing C code, strictly adhere to these standards:

## 1. Standard & Portability
- **Language Standard:** Follow ISO C99 / C11 standards.
- **Fixed-Width Types:** Prefer explicit fixed-width integer types from `<stdint.h>` (`uint8_t`, `int16_t`, `uint32_t`, `int64_t`, `uintptr_t`, `size_t`) over ambiguous native types (`int`, `long`, `short`) when specifying memory layouts, data buffers, or bit masks.
- **Boolean Logic:** Use `<stdbool.h>` (`bool`, `true`, `false`) for logical states.

## 2. Pointers & Memory Safety
- **Pointer Initialization:** Always initialize pointers to a valid address or `NULL`.
- **Null Checks:** Check pointers for `NULL` before dereferencing, especially pointers passed as function arguments or returned by allocation functions.
- **Dangling Pointers:** Set pointers to `NULL` immediately after freeing dynamic memory (`free(ptr); ptr = NULL;`).
- **Array Bounds & Buffer Safety:** Always pass the length/capacity alongside pointer buffers to avoid buffer overflow vulnerabilities. Prefer `snprintf` over `sprintf` or `strcpy`.

## 3. Bitwise & Low-Level Manipulation
- **Explicit Unsigned Types:** Always use unsigned types (`uint8_t`, `uint16_t`, `uint32_t`) when performing bitwise operations (`&`, `|`, `^`, `~`, `<<`, `>>`) to prevent undefined behavior from signed overflow or sign extension.
- **Clear Mask Definitions:** Define masks with explicit unsigned constants or bit-shift macros (e.g., `#define BIT(n) (1U << (n))`).

## 4. Structs, Unions, & Memory Layout
- **Alignment & Padding Awareness:** Understand compiler padding and alignment in struct definitions. Order fields by descending size when optimizing memory footprint.
- **Type Definitions:** Use clear `typedef` idioms (e.g., `typedef struct { ... } sensor_data_t;`).

## 5. Clean Code & Const Correctness
- **Const Correctness:** Mark read-only pointer parameters as `const` (e.g., `size_t string_length(const char *str)`).
- **Scope Limitation:** Declare variables in the smallest applicable scope. Use `static` for functions and file-scoped variables that do not need external linkage.
- **Compilation Cleanliness:** Code must compile cleanly with `-Wall -Wextra -Wpedantic` with zero warnings.
