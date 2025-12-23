# raylib-hemlock

Raylib bindings for the Hemlock programming language using FFI.

## Requirements

- Hemlock interpreter
- raylib 5.0+

### Installing raylib (macOS)

```bash
brew install raylib
```

### Installing raylib (Linux)

```bash
# Ubuntu/Debian
sudo apt install libraylib-dev

# Or build from source
git clone https://github.com/raysan5/raylib.git
cd raylib/src
make PLATFORM=PLATFORM_DESKTOP
sudo make install
```

## Usage

```hemlock
import "/opt/homebrew/opt/raylib/lib/libraylib.dylib";

extern fn InitWindow(width: i32, height: i32, title: string): void;
extern fn CloseWindow(): void;
extern fn WindowShouldClose(): i32;
extern fn BeginDrawing(): void;
extern fn EndDrawing(): void;
extern fn ClearBackground(color: u32): void;
extern fn DrawText(text: string, x: i32, y: i32, size: i32, color: u32): void;

// Color helper (ABGR format for little-endian)
fn Color(r: u8, g: u8, b: u8, a: u8): u32 {
    let r32: u32 = r;
    let g32: u32 = g;
    let b32: u32 = b;
    let a32: u32 = a;
    return (a32 << 24) | (b32 << 16) | (g32 << 8) | r32;
}

let WHITE = Color(255, 255, 255, 255);
let BLACK = Color(0, 0, 0, 255);

InitWindow(800, 450, "Hello Raylib!");

while (WindowShouldClose() == 0) {
    BeginDrawing();
    ClearBackground(WHITE);
    DrawText("Hello from Hemlock!", 300, 200, 20, BLACK);
    EndDrawing();
}

CloseWindow();
```

## Running Examples

```bash
cd raylib-hemlock
hemlock examples/hello_window.hml
hemlock examples/input_demo.hml
```

## Project Structure

```
raylib-hemlock/
├── src/
│   └── raylib.hml         # Core raylib bindings (reference)
├── examples/
│   ├── hello_window.hml   # Basic window + shapes + text
│   └── input_demo.hml     # Keyboard + mouse input
└── README.md
```

## Available Bindings

### Window Management
- `InitWindow`, `CloseWindow`, `WindowShouldClose`
- `SetTargetFPS`, `GetFPS`, `GetFrameTime`, `GetTime`
- `GetScreenWidth`, `GetScreenHeight`
- `ToggleFullscreen`, `MaximizeWindow`, `MinimizeWindow`

### Drawing
- `BeginDrawing`, `EndDrawing`, `ClearBackground`
- `DrawPixel`, `DrawLine`, `DrawCircle`, `DrawCircleLines`
- `DrawRectangle`, `DrawRectangleLines`
- `DrawText`, `MeasureText`

### Input
- `IsKeyPressed`, `IsKeyDown`, `IsKeyReleased`, `IsKeyUp`
- `GetKeyPressed`, `GetCharPressed`
- `IsMouseButtonPressed`, `IsMouseButtonDown`, `IsMouseButtonReleased`
- `GetMouseX`, `GetMouseY`, `GetMouseWheelMove`
- `ShowCursor`, `HideCursor`

## Status

Working - core window, drawing, and input functions are available.
