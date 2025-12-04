# bun-glu32

Zero-dependency, zero-overhead Win32 GLU32 bindings for [Bun](https://bun.sh) on Windows.

## Overview

`bun-glu32` exposes every entry point exported by `glu32.dll` through Bun's `ffi` layer. The package exposes a single, lazy-loading `GLU32` class that mirrors the native API in both names and signatures. Callers can rely on the built-in memoized getters, or eagerly bind a subset or the full export table using `GLU32.Preload()`.

Symbols are declared alphabetically, typed with `FFIType`, and documented directly above each member in `structs/GLU32.ts`, so you get Bun-native ergonomics without wrappers.

## Features

- Bun-first ergonomics tuned for Windows 10/11.
- Direct FFI to `glu32.dll` (quadrics, tessellation, NURBS, sampling, and matrix helpers).
- In-source docs (each method links to the corresponding Microsoft Docs entry in `structs/GLU32.ts`).
- Lazy binding on first call; optional eager preload (`GLU32.Preload()`).
- No wrapper overhead; every helper is a 1:1 Win32 call.
- Strongly typed GLU aliases and structs (see `types/GLU32.ts`).

## Requirements

- [Bun](https://bun.sh) runtime
- Windows 10 or later

## Installation

```sh
bun add bun-glu32
```

## Quick Start

```ts
import GLU32 from 'bun-glu32';

GLU32.Preload(['gluSphere', 'gluDisk']);

// GLU32.gluNewQuadric() will be loaded here
const quadric = GLU32.gluNewQuadric();

// GLU32.gluSphere() was already loaded
GLU32.gluSphere(quadric, 1.0, 32, 32);

// GLU32.gluDeleteQuadric() will be loaded here
GLU32.gluDeleteQuadric(quadric);
```

## API Highlights

- `Preload()` loads and caches any subset of exports you need before the first call.
- `gluNewQuadric`, `gluDeleteQuadric`, `gluSphere`, `gluCylinder`, and `gluDisk` for quadric drawing.
- Tessellation helpers such as `gluNewTess`, `gluTessBeginPolygon`, `gluTessCallback`, and `gluTessVertex`.
- Matrix and sampling helpers like `gluLookAt`, `gluPerspective`, `gluPickMatrix`, and `gluProject`.
- Buffer-friendly `.ptr` helpers provided by Bun for ArrayBuffer/Buffer/DataView/TypedArray make pointer arguments ergonomic.

## Examples

Run the basic example:

```sh
bun run example
```

Run the comprehensive demo (queries GLU info, creates quadrics/tessellators/NURBS, tests projection):

```sh
bun run demo
```

## Notes

- No global initialization is required. Use lazy binding or call `GLU32.Preload()` to bind everything sooner.
- All GLU helpers expect native floating-point types; `types/GLU32.ts` exposes the precise aliases.
- Bun runtime on Windows is mandatory.
