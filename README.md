# ElectronicStoreMod

A minimal C++ DLL project scaffold for building Windows DLL mods (e.g. Electronic Store mod).  
Uses **CMake + Ninja** with **MSVC** and targets **C++23**. Designed to be copied and reused as a starting point (scaffolding) for new DLL projects.

---

## Project Structure

```
ElectronicStoreMod/
├── CMakeLists.txt          # CMake project definition
├── CMakePresets.json       # CMake configure presets (x64 Debug / Release)
├── dllmain.cpp             # DLL entry point (DllMain)
└── README.md               # This file
```

---

## Prerequisites

- **Windows** (target platform)
- **CMake 3.10+** (3.12+ recommended for C++23 enforcement)
- **Ninja** build generator
- **MSVC compiler** (`cl.exe`) — via Visual Studio Build Tools or Visual Studio
- A Developer Command Prompt for VS (or ensure `cl.exe`, `cmake.exe`, and `ninja.exe` are on your `PATH`)

Verify your toolchain:

```cmd
cmake --version
ninja --version
cl
```

---

## Build Commands (CLI)

### Option 1: Using CMake Presets (recommended)

Configure:

```cmd
# From the project root
cmake --preset x64-debug
# or
cmake --preset x64-release
```

Build:

```cmd
cmake --build --preset x64-debug
# or
cmake --build --preset x64-release
```

The output DLL will be placed under:

```
out/build/x64-debug/ElectronicStore.dll   # Debug
out/build/x64-release/ElectronicStore.dll # Release
```

Install (optional):

```cmd
cmake --install --preset x64-release
# Output goes to: out/install/x64-release/
```

### Option 2: Plain CMake CLI (no presets)

From a Developer Command Prompt:

```cmd
# Create build directory
mkdir out\build
cd out\build

# Configure (x64, Ninja, MSVC)
cmake -G Ninja -A x64 -DCMAKE_BUILD_TYPE=Release ..\..

# Build
cmake --build . --config Release

# The DLL is produced in the current directory
dir ElectronicStore.dll
```

### Option 3: One-liner (configure + build)

```cmd
cmake --preset x64-release && cmake --build --preset x64-release
```

---

## Clean

```cmd
# Remove generated build/install output
rmdir /s /q out
```

---

## Using This as Scaffolding

This repository is intended to be copied and adapted for new DLL mod projects.

### 1. Copy the scaffold

```cmd
xcopy /E /I ElectronicStoreMod MyNewMod
```

### 2. Rename the project

Edit `CMakeLists.txt` and change the project/library name:

```cmake
project("MyNewMod")

add_library(MyNewMod SHARED "dllmain.cpp")
```

Also rename the output target if desired:

```cmake
set_property(TARGET MyNewMod PROPERTY OUTPUT_NAME MyNewMod)
```

### 3. Update the entry point

Edit `dllmain.cpp` to add your mod initialization logic inside `DllMain` (typically under `DLL_PROCESS_ATTACH`):

```cpp
#include <windows.h>

BOOL APIENTRY DllMain(HMODULE hModule, DWORD ul_reason_for_call, LPVOID lpReserved)
{
    switch (ul_reason_for_call)
    {
    case DLL_PROCESS_ATTACH:
        // Your mod initialization here
        break;
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}
```

> ⚠️ Keep `DllMain` lightweight. Avoid heavy initialization, locks, or `LoadLibrary` calls inside `DllMain`. Defer complex setup to a separate thread or delayed initializer.

### 4. Build the new mod

```cmd
cd MyNewMod
cmake --preset x64-release
cmake --build --preset x64-release
```

### 5. Iterate

- Add source files to `add_library(... SHARED ...)` in `CMakeLists.txt`.
- Adjust `CXX_STANDARD` if you need a different C++ version.
- Add dependencies via `find_package`, `target_link_libraries`, or `FetchContent` as needed.

---

## License

Unspecified. Add a `LICENSE` file if you intend to distribute this scaffold or derived mods.
