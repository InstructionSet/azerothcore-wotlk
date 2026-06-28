# AzerothCore – Copilot Instructions

## Directory Layout

```
D:/Data/AC/
├── src/     ← source tree (CMakeLists.txt root)
└── build/   ← out-of-source build directory
```

Source and build directories are **siblings**, not nested.

## Build Commands (Windows)

### Reconfigure + Build (only when CMake changes are needed)

Run CMake reconfigure when:

- New `.cpp` or `.h` files were added or removed
- A module was added or removed under `modules/`
- Any `CMakeLists.txt` was modified
- CMake options need changing

```powershell
cmake -S D:/Data/AC/src -B D:/Data/AC/build
cmake --build D:/Data/AC/build -j<cores>
```

### Build only (no reconfigure needed)

When only editing existing `.cpp`/`.h` files, skip cmake and just build:

```powershell
cmake --build D:/Data/AC/build -j<cores>
```

### Install after build

```powershell
cmake --install D:/Data/AC/build
```

## Key CMake Options

| Option          | Values                                                 | Default |
| --------------- | ------------------------------------------------------ | ------- |
| `SCRIPTS`       | none, static, dynamic, minimal-static, minimal-dynamic | static  |
| `MODULES`       | none, static, dynamic                                  | static  |
| `APPS_BUILD`    | none, all, auth-only, world-only                       | all     |
| `TOOLS_BUILD`   | none, all, db-only, maps-only                          | none    |
| `BUILD_TESTING` | ON/OFF                                                 | OFF     |

To change an option without full reconfigure:

```powershell
cmake -S D:/Data/AC/src -B D:/Data/AC/build -DSCRIPTS=static -DMODULES=static
```

## General Rules

- **Skip building unless explicitly requested.**
- Never use `cmake ..` — the build dir is beside `src/`, not inside it.
- Do not modify files in `data/sql/updates/` outside of `pending_*` subdirectories.
- SQL pending files use random names.
- All C++ changes: 4-space indentation, LF line endings, max 80 char lines.
