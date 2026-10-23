# Ray Tracing in C++

An educational C++ implementation of ray tracing and stochastic image synthesis.
The project explores geometric intersections, recursive ray propagation, camera
models, and pixel sampling.

Its organization and rendering methods closely follow *Ray Tracing in One
Weekend*. The project should be understood as a learning implementation rather
than a novel rendering algorithm.

## Implementation and Branches

The earlier `Working` revision at `67cf1c4` includes:

- A three-dimensional vector and ray representation.
- Analytic ray–sphere intersections.
- Nearest-hit selection through a linear object list.
- Recursive hemisphere scattering with a configurable depth limit.
- Jittered pixel sampling and averaged color output.
- Plain-text PPM image export.

A later implementation is present on `master`, based on revision `eb370ee`. It
adds diffuse, metallic, and dielectric materials, reflection and refraction,
Schlick reflectance, gamma correction, an adjustable camera, and depth of field.
Those features are not all present in the earlier `Working` revision.

Check the selected revision before interpreting a rendered image or comparing
results. Features from different revisions should not be attributed to a single
build without verifying its source.

## Build

The repository includes a minimal CMake definition:

~~~powershell
cmake -S . -B build
cmake --build build --config Release
~~~

The executable location depends on the selected CMake generator. Alternatively,
with a compatible GCC toolchain:

~~~powershell
g++ -O2 -std=c++17 main.cpp -o raytracing.exe
~~~

## Rendering

The scene and camera settings are defined in `main.cpp`. Image data are written
to standard output; progress messages are written to standard error.

With the directly compiled Windows executable, preserve native text output by
redirecting it through the command processor:

~~~powershell
cmd /c ".\raytracing.exe > image.ppm"
~~~

This avoids the text-encoding conversions applied by some PowerShell versions.
Open the resulting file in a viewer that supports PPM images.

Before a longer render, review image width, samples per pixel, recursion depth,
and scene complexity. These settings substantially affect execution time.

## Structure

| File | Purpose |
| --- | --- |
| `main.cpp` | Scene construction and rendering configuration |
| `camera.h` | Camera setup, sampling, and recursive color evaluation |
| `sphere.h` | Ray–sphere intersection |
| `hittable.h` | Intersection interface and hit records |
| `hittable_list.h` | Linear scene traversal |
| `ray.h`, `vec3.h` | Ray and vector operations |
| `color.h` | Pixel conversion and output |
| `interval.h` | Ray-parameter and output bounds |
| `utilities.h` | Shared constants and random sampling |

The later material-based revision also includes `material.h`.

## Scope and Limitations

Scene traversal is linear in the number of objects. No bounding-volume hierarchy,
parallel renderer, or GPU rendering implementation is included in the inspected
revisions. Rendering settings describe computational effort; they do not
establish image convergence or measured performance on another machine.
