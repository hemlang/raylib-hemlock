# Claude Instructions for raylib-hemlock

## Getting Hemlock

This project requires the Hemlock programming language interpreter. To get Hemlock:

```bash
# Clone the Hemlock repository
git clone https://github.com/hemlang/hemlock.git

# Build Hemlock (follow instructions in the hemlock repo)
cd hemlock
# Build commands depend on the hemlock project structure - check its README
```

## Project Overview

This is a raylib bindings library for Hemlock using FFI. The main bindings are in `src/raylib.hml`.

## Running Examples

Examples require:
1. Hemlock interpreter installed and in PATH
2. raylib library installed on the system

```bash
hemlock examples/hello_window.hml
hemlock examples/input_demo.hml
hemlock examples/animation.hml
```

## Running Tests

```bash
./tests/run_tests.sh /path/to/hemlock
```

## Key Files

- `src/raylib_loader.hml` - Cross-platform raylib library loader (macOS + Linux)
- `src/raylib.hml` - Core raylib FFI bindings and utility functions
- `examples/` - Example Hemlock programs using raylib
- `tests/` - Test suite for utility functions

## Cross-Platform Support

The `src/raylib_loader.hml` module automatically detects the platform and loads raylib from common installation paths:

- **macOS (Apple Silicon)**: `/opt/homebrew/opt/raylib/lib/libraylib.dylib`
- **macOS (Intel)**: `/usr/local/lib/libraylib.dylib`
- **Linux**: `/usr/lib/libraylib.so`, `/usr/local/lib/libraylib.so`

Examples import the loader first, then define their extern functions.

## Hemlock Language Quirks

When writing Hemlock code for raylib, be aware of these quirks:

### No Block Scoping
Variables declared with `let` inside `if` blocks or `while` loops remain in scope for the rest of the function. You cannot redeclare a variable with the same name later in the function.

```hemlock
// BAD - will error "Variable 'i' already defined"
if (condition != 0) {
    let i = 0;
    // ...
}
let i = 0;  // Error!

// GOOD - use different variable names
if (condition != 0) {
    let i = 0;
    // ...
}
let j = 0;  // OK
```

### Function Return Type Syntax
Use `: type` not `-> type` for return types:

```hemlock
// GOOD
fn myFunc(x: i32): i32 {
    return x * 2;
}

// BAD - will not parse
fn myFunc(x: i32) -> i32 {
    return x * 2;
}
```

### String Type
Use `string` not `str` for string types.

### DrawCircle/DrawEllipse Issues
`DrawCircle` and `DrawEllipse` may not render correctly when positions are calculated from `f32` variables, even after casting to `i32`. Prefer using `DrawRectangle` and `DrawTriangle` for reliable rendering when working with dynamic positions.

```hemlock
// May not render:
let x: f32 = 100.0;
let ix: i32 = x;
DrawCircle(ix, 200, 10.0, RED);

// Reliable alternative:
DrawRectangle(ix - 10, 190, 20, 20, RED);
```

### Type Mixing in Arithmetic
Be careful mixing `i32` and `f32` in arithmetic expressions passed directly to draw functions. Convert to the expected type first:

```hemlock
// Safer approach
let posX: i32 = myFloatX;  // Convert once
let posY: i32 = myFloatY;
DrawRectangle(posX - 10, posY - 10, 20, 20, RED);
```

### Available Colors
These colors are defined: `LIGHTGRAY`, `GRAY`, `DARKGRAY`, `YELLOW`, `GOLD`, `ORANGE`, `PINK`, `RED`, `MAROON`, `GREEN`, `LIME`, `DARKGREEN`, `SKYBLUE`, `BLUE`, `DARKBLUE`, `PURPLE`, `VIOLET`, `DARKPURPLE`, `BEIGE`, `BROWN`, `DARKBROWN`, `WHITE`, `BLACK`, `BLANK`, `MAGENTA`, `RAYWHITE`

Note: `CYAN` is NOT defined - use `SKYBLUE` instead.
