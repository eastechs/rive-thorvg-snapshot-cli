# riv-render

Render a frame from a self-contained `.riv` file on a computer. Choose an artboard, viewport and playback time, then write a PPM image or raw RGB565 pixels. The command uses the [Rive framebuffer renderer](deps/rive-framebuffer) and runs independently of ESP-IDF and any display hardware.

## Build

The first-party submodule URLs resolve to sibling repositories on the same Git host. When cloning from trusted local directories, use `git -c protocol.file.allow=always submodule update --init --recursive` to initialize them.

Use Git, CMake 3.20+, a C/C++17 toolchain and the Unix `patch` utility. The output checks use Python 3. Configure with `-DBUILD_TESTING=OFF` to build just the command.

```sh
git submodule update --init --recursive --depth 1
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel
ctest --test-dir build --output-on-failure
```

## Render

```sh
build/riv-render scene.riv preview.ppm --artboard "Dial" --time 1
build/riv-render scene.riv frame.rgb565 --size 466x466
```

Omit `--artboard` to use the default artboard. The default size is 233 × 233 and the default time is zero seconds. The scene is centered and fitted inside the viewport. Animated scenes advance in steps of up to 1/60 second before the snapshot. Static artboards render a still.

PPM output contains 8-bit RGB. RGB565 output is tightly packed in row-major, little-endian order. Transparent pixels composite against black. Use embedded assets and outlined lettering with this renderer. To install the executable, run `cmake --install build --prefix /your/install/directory`.

Original CLI code is MIT licensed. [License](LICENSE) · [Renderer and upstream credits](deps/rive-framebuffer)
