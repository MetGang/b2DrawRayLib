# b2DrawSFML

Implementation of Box2D's b2Draw class for RayLib.

## Dependencies

* C++11 capable compiler
* [RayLib](https://github.com/raysan5/raylib) (tested on 4.2.0)
* [Box2D](https://github.com/erincatto/box2d) (tested on 2.4.1)

## Usage

Please refer to the example below.

```cpp
// b2DrawRayLib
#include <b2DrawRayLib.hpp>

// Box2D
#include <box2d/box2d.h>

// RayLib
#include <raylib.h>

void CreateScene(b2World& world)
{
    // Crate your scene with all objects here
}

int main()
{
    // Create RayLib window for application
    InitWindow(1280, 720, "b2DrawRayLib example");

    // Limit framerate for sake of consistency
    SetTargetFPS(60);

    // Create debug drawer for window with 10x scale
    b2DrawRayLib drawer{ { 10.0f, 10.0f } };

    // Set flags for things that should be drawn
    // ALWAYS rememeber to set at least one flag
    // otherwise nothing will be drawn
    drawer.SetFlags(
        b2Draw::e_shapeBit |
        b2Draw::e_jointBit |
        b2Draw::e_aabbBit |
        b2Draw::e_pairBit |
        b2Draw::e_centerOfMassBit
    );

    // Create Box2D world
    b2World world{ { 0.0f, 9.80665f } };

    // Set our drawer as world's drawer
    world.SetDebugDraw(&drawer);

    // Create scene with all objects
    CreateScene(world);

    while (!WindowShouldClose())
    {
        // Calculate delta time as float seconds
        auto const dt = GetFrameTime();

        // Calculate physics
        world.Step(dt, 6, 2);

        // Begin drawing
        BeginDrawing();

        // Clear window
        ClearBackground(BLACK);

        // Draw debug shapes of all physics objects
        world.DebugDraw();

        // End drawing (display window content)
        EndDrawing();
    }

    CloseWindow();

    return 0;
}
```

## Scaling

To ensure best precision Box2D uses units called *meters* which is just a fancy name for small floats because floating-point numbers lose precision when the number gets bigger. That's why it's the best to keep size of your physics object in range from 0.0f to 10.0f and then use scale while rendering.

## License

MIT, see [license](/LICENSE) for more information.
