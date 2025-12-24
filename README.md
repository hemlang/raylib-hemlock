# raylib-hemlock

Raylib bindings for the Hemlock programming language using FFI.

## Requirements

- [Hemlock](https://github.com/hemlang/hemlock) interpreter
- [raylib](https://www.raylib.com/) 5.0+

### Installing raylib

**macOS (Homebrew)**
```bash
brew install raylib
```

**Linux (Ubuntu/Debian)**
```bash
sudo apt install libraylib-dev
```

**Linux (From Source)**
```bash
git clone https://github.com/raysan5/raylib.git
cd raylib/src
make PLATFORM=PLATFORM_DESKTOP
sudo make install
```

## Quick Start

```hemlock
// Load raylib library - adjust path for your system:
//   Linux: import "/usr/lib/libraylib.so";
//   macOS (Apple Silicon): import "/opt/homebrew/opt/raylib/lib/libraylib.dylib";
//   macOS (Intel): import "/usr/local/lib/libraylib.dylib";
import "/usr/lib/libraylib.so";

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

# Adjust library path in examples for your system first
hemlock examples/hello_window.hml
hemlock examples/input_demo.hml
hemlock examples/animation.hml
```

## Running Tests

The test suite verifies the utility functions (collision detection, math helpers, color functions):

```bash
# Run all tests
./tests/run_tests.sh /path/to/hemlock

# Or run individual tests
hemlock tests/test_colors.hml
hemlock tests/test_collision.hml
hemlock tests/test_math.hml
hemlock tests/test_constants.hml
```

## Project Structure

```
raylib-hemlock/
├── src/
│   └── raylib.hml               # Core raylib bindings and utilities
├── examples/
│   ├── hello_window.hml         # Basic window, shapes, and text
│   ├── input_demo.hml           # Keyboard and mouse input
│   └── animation.hml            # Bouncing balls with physics
├── tests/
│   ├── run_tests.sh             # Test runner script
│   ├── test_colors.hml          # Basic color function tests
│   ├── test_colors_extended.hml # Extended color tests (blend, invert, etc.)
│   ├── test_collision.hml       # Basic collision detection tests
│   ├── test_collision_extended.hml # Extended collision tests (lines, triangles)
│   ├── test_math.hml            # Basic math utility tests
│   ├── test_math_extended.hml   # Extended math tests
│   ├── test_vectors.hml         # Vector operation tests
│   ├── test_easing.hml          # Easing function tests
│   └── test_constants.hml       # Constant value tests
├── .gitignore
└── README.md
```

## Available Bindings

### Window Management
- `InitWindow`, `CloseWindow`, `WindowShouldClose`
- `SetTargetFPS`, `GetFPS`, `GetFrameTime`, `GetTime`
- `GetScreenWidth`, `GetScreenHeight`, `GetRenderWidth`, `GetRenderHeight`
- `GetMonitorCount`, `GetCurrentMonitor`, `GetMonitorWidth`, `GetMonitorHeight`
- `ToggleFullscreen`, `ToggleBorderlessWindowed`
- `MaximizeWindow`, `MinimizeWindow`, `RestoreWindow`
- `SetWindowTitle`, `SetWindowPosition`, `SetWindowSize`
- `SetWindowMinSize`, `SetWindowMaxSize`, `SetWindowOpacity`

### Drawing
- `BeginDrawing`, `EndDrawing`, `ClearBackground`
- `BeginBlendMode`, `EndBlendMode`
- `BeginScissorMode`, `EndScissorMode`
- `DrawPixel`, `DrawLine`, `DrawLineEx`
- `DrawCircle`, `DrawCircleGradient`, `DrawCircleLines`
- `DrawEllipse`, `DrawEllipseLines`
- `DrawRectangle`, `DrawRectangleGradientV`, `DrawRectangleGradientH`, `DrawRectangleLines`
- `DrawTriangle`, `DrawTriangleLines`
- `DrawText`, `MeasureText`

### Input - Keyboard
- `IsKeyPressed`, `IsKeyPressedRepeat`, `IsKeyDown`, `IsKeyReleased`, `IsKeyUp`
- `GetKeyPressed`, `GetCharPressed`, `SetExitKey`

### Input - Mouse
- `IsMouseButtonPressed`, `IsMouseButtonDown`, `IsMouseButtonReleased`, `IsMouseButtonUp`
- `GetMouseX`, `GetMouseY`, `GetMouseWheelMove`
- `SetMousePosition`, `SetMouseOffset`, `SetMouseScale`
- `ShowCursor`, `HideCursor`, `IsCursorHidden`
- `EnableCursor`, `DisableCursor`, `IsCursorOnScreen`

### Input - Gamepad
- `IsGamepadAvailable`, `GetGamepadName`
- `IsGamepadButtonPressed`, `IsGamepadButtonDown`, `IsGamepadButtonReleased`, `IsGamepadButtonUp`
- `GetGamepadButtonPressed`, `GetGamepadAxisCount`, `GetGamepadAxisMovement`
- `SetGamepadMappings`

### Input - Touch & Gestures
- `GetTouchX`, `GetTouchY`, `GetTouchPointId`, `GetTouchPointCount`
- `SetGesturesEnabled`, `IsGestureDetected`, `GetGestureDetected`
- `GetGestureHoldDuration`, `GetGestureDragAngle`, `GetGesturePinchAngle`

### Audio - Device
- `InitAudioDevice`, `CloseAudioDevice`, `IsAudioDeviceReady`
- `SetMasterVolume`, `GetMasterVolume`

### Audio - Sound
- `LoadSound`, `UnloadSound`
- `PlaySound`, `StopSound`, `PauseSound`, `ResumeSound`
- `IsSoundPlaying`, `SetSoundVolume`, `SetSoundPitch`, `SetSoundPan`

### Audio - Music
- `LoadMusicStream`, `UnloadMusicStream`
- `PlayMusicStream`, `StopMusicStream`, `PauseMusicStream`, `ResumeMusicStream`
- `UpdateMusicStream`, `IsMusicStreamPlaying`
- `SetMusicVolume`, `SetMusicPitch`, `SetMusicPan`
- `GetMusicTimeLength`, `GetMusicTimePlayed`, `SeekMusicStream`, `SetMusicLooping`

### Textures
- `LoadTexture`, `UnloadTexture`, `LoadTextureFromImage`
- `DrawTexture`, `DrawTextureV`, `DrawTextureEx`, `DrawTextureRec`, `DrawTexturePro`
- `GetTextureWidth`, `GetTextureHeight`
- `SetTextureFilter`, `SetTextureWrap`, `GenTextureMipmaps`
- `LoadRenderTexture`, `UnloadRenderTexture`, `BeginTextureMode`, `EndTextureMode`

### Fonts
- `LoadFont`, `LoadFontEx`, `UnloadFont`
- `DrawTextEx`, `DrawTextPro`, `MeasureTextEx`
- `GetFontDefault`, `IsFontValid`

### Images
- `LoadImage`, `LoadImageRaw`, `LoadImageFromMemory`, `UnloadImage`, `ExportImage`
- `GenImageColor`, `GenImageGradientLinear`, `GenImageGradientRadial`, `GenImageChecked`, `GenImageWhiteNoise`, `GenImagePerlinNoise`
- `ImageCopy`, `ImageFromImage`, `ImageResize`, `ImageResizeNN`, `ImageCrop`
- `ImageFlipVertical`, `ImageFlipHorizontal`, `ImageRotate`, `ImageRotateCW`, `ImageRotateCCW`
- `ImageColorTint`, `ImageColorInvert`, `ImageColorGrayscale`, `ImageColorContrast`, `ImageColorBrightness`
- `ImageClearBackground`, `ImageDrawPixel`, `ImageDrawLine`, `ImageDrawCircle`, `ImageDrawRectangle`, `ImageDrawText`
- `GetImageWidth`, `GetImageHeight`

### File I/O
- `FileExists`, `DirectoryExists`, `IsFileExtension`, `IsPathFile`
- `GetFileExtension`, `GetFileName`, `GetFileNameWithoutExt`
- `GetDirectoryPath`, `GetPrevDirectoryPath`, `GetWorkingDirectory`, `ChangeDirectory`
- `GetFileLength`, `GetFileModTime`
- `LoadFileText`, `UnloadFileText`, `SaveFileText`

### Utility Functions (Pure Hemlock)
- **Colors**: `Color`, `ColorAlpha`, `ColorBrightness`, `ColorGetR/G/B/A`
- **Collision**: `CheckCollisionCirclesXY`, `CheckCollisionRectsXY`, `CheckCollisionPointRectXY`, `CheckCollisionPointCircleXY`, `CheckCollisionCircleRecXY`
- **Math**: `Vector2Distance`, `Vector2Length`, `Vector2Dot`, `Lerp`, `Clamp`, `ClampInt`, `Remap`, `Wrap`, `NormalizeAngle`, `Deg2Rad`, `Rad2Deg`

### Constants
- **Colors**: `LIGHTGRAY`, `GRAY`, `DARKGRAY`, `YELLOW`, `GOLD`, `ORANGE`, `PINK`, `RED`, `MAROON`, `GREEN`, `LIME`, `DARKGREEN`, `SKYBLUE`, `BLUE`, `DARKBLUE`, `PURPLE`, `VIOLET`, `DARKPURPLE`, `BEIGE`, `BROWN`, `DARKBROWN`, `WHITE`, `BLACK`, `BLANK`, `MAGENTA`, `RAYWHITE`
- **Keys**: `KEY_A`-`KEY_Z`, `KEY_ZERO`-`KEY_NINE`, `KEY_SPACE`, `KEY_ESCAPE`, `KEY_ENTER`, arrow keys, function keys, modifiers, etc.
- **Mouse**: `MOUSE_BUTTON_LEFT`, `MOUSE_BUTTON_RIGHT`, `MOUSE_BUTTON_MIDDLE`, etc.
- **Gamepad**: Button and axis constants
- **Blend Modes**: `BLEND_ALPHA`, `BLEND_ADDITIVE`, `BLEND_MULTIPLIED`, etc.
- **Gestures**: `GESTURE_TAP`, `GESTURE_HOLD`, `GESTURE_DRAG`, swipes, pinch, etc.
- **Texture Filters**: `TEXTURE_FILTER_POINT`, `TEXTURE_FILTER_BILINEAR`, `TEXTURE_FILTER_TRILINEAR`, etc.
- **Texture Wrap**: `TEXTURE_WRAP_REPEAT`, `TEXTURE_WRAP_CLAMP`, `TEXTURE_WRAP_MIRROR_REPEAT`, etc.
- **Pixel Formats**: `PIXELFORMAT_UNCOMPRESSED_GRAYSCALE`, `PIXELFORMAT_UNCOMPRESSED_R8G8B8A8`, etc.

## Notes

### Library Path
Each example needs the raylib library path adjusted for your system. Common paths:
- **Linux**: `/usr/lib/libraylib.so` or `/usr/local/lib/libraylib.so`
- **macOS (Apple Silicon)**: `/opt/homebrew/opt/raylib/lib/libraylib.dylib`
- **macOS (Intel)**: `/usr/local/lib/libraylib.dylib`

### Color Format
Raylib uses RGBA colors packed as 32-bit integers. On little-endian systems (most modern computers), the bytes are stored as ABGR in memory. The `Color()` helper function handles this conversion.

### Struct Limitations
Since Hemlock's FFI currently passes structs by value for small structs but doesn't have full struct support, some raylib functions that require complex structs (like `Vector2`, `Rectangle`, `Camera2D`) have coordinate-based alternatives provided in the bindings.

### Textures, Sounds, Fonts, and Images
These resources are returned as opaque pointers (`ptr` type) from the load functions. Make sure to:
1. Always call the corresponding `Unload*` function when done to free memory
2. For music streams, call `UpdateMusicStream` in your game loop to keep audio playing
3. Textures require a valid OpenGL context (window must be initialized first)

## License

MIT License - see raylib's license for the raylib library itself.
