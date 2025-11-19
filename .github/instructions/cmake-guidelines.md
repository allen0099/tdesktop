---
applyTo: "**/CMakeLists.txt,**/*.cmake"
---

# CMake Build System Guidelines

## Build System Overview

This project uses CMake (minimum version 3.25) for cross-platform builds:
- Windows: Visual Studio 2022 with Windows SDK 10.0.26100.0
- Linux: Docker-based CentOS environment
- macOS: Native build tools

## Modifying CMake Files

### General Principles
- Maintain compatibility with CMake 3.25+
- Test changes on all target platforms if possible
- Keep build scripts simple and maintainable
- Follow existing patterns in the CMakeLists.txt files

### Common CMake Patterns in This Project

#### Version Management
```cmake
include(cmake/version.cmake)
desktop_app_parse_version(Telegram/build/version)
```

#### Adding Source Files
```cmake
include(cmake/nice_target_sources.cmake)
nice_target_sources(TargetName ${src_loc}
    file1.cpp
    file2.cpp
    file2.h
)
```

#### Platform-Specific Code
```cmake
if (WIN32)
    # Windows-specific
elseif (APPLE)
    # macOS-specific
elseif (LINUX)
    # Linux-specific
endif()
```

#### Qt Integration
```cmake
include(cmake/external/qt/package.cmake)
# Qt is handled through the package system
```

### Dependencies
- This project has many dependencies in `Telegram/ThirdParty/` and `lib/`
- Submodules are used extensively - ensure they're initialized
- Don't add new third-party dependencies without strong justification
- Document any new dependencies clearly

### Build Targets
- Main target is `Telegram`
- Multiple helper libraries are built as dependencies
- Use `add_subdirectory()` for modular builds

### Configuration Options
```cmake
# API credentials (required for building)
-D TDESKTOP_API_ID=YOUR_API_ID
-D TDESKTOP_API_HASH=YOUR_API_HASH

# Build type
-D CMAKE_BUILD_TYPE=Debug  # or Release
```

### Testing Build Changes

#### Windows
```bash
# From x64 Native Tools Command Prompt for VS 2022
cd Telegram
configure.bat x64 -D TDESKTOP_API_ID=XXX -D TDESKTOP_API_HASH=YYY
# Open out\Telegram.sln and build
```

#### Linux
```bash
docker run --rm -it -u $(id -u) \
    -v "$PWD:/usr/src/tdesktop" \
    tdesktop:centos_env \
    /usr/src/tdesktop/Telegram/build/docker/centos_env/build.sh \
    -D TDESKTOP_API_ID=XXX -D TDESKTOP_API_HASH=YYY
```

#### macOS
```bash
# Follow docs/building-mac.md
```

### Common Tasks

#### Adding a New Source File
1. Add to the appropriate `CMakeLists.txt`
2. Use `nice_target_sources()` helper
3. Maintain alphabetical order if that's the pattern in that file
4. Include both `.cpp` and `.h` files

#### Adding a Compile Definition
```cmake
target_compile_definitions(Telegram PRIVATE
    NEW_DEFINITION=value
)
```

#### Linking a Library
```cmake
target_link_libraries(Telegram
    PRIVATE
        external::library_name
)
```

### What NOT to Change
- Don't modify version parsing logic unless absolutely necessary
- Don't change minimum CMake version without testing thoroughly
- Don't alter Qt package integration without deep understanding
- Don't break cross-platform compatibility

### Debugging Build Issues
- Check CMake cache: `cmake -L out`
- Verbose build: `cmake --build out --verbose`
- Clean rebuild: `rm -rf out && reconfigure`
- Check CMake version: `cmake --version`

### Build Scripts Location
- `Telegram/build/prepare/win.bat` - Windows preparation
- `Telegram/build/prepare/linux.sh` - Linux preparation
- `Telegram/build/docker/` - Docker build environment
- `cmake/` - CMake helper modules

### Important CMake Variables
- `TDESKTOP_API_ID` - Required API ID
- `TDESKTOP_API_HASH` - Required API hash
- `CMAKE_BUILD_TYPE` - Debug or Release
- `DESKTOP_APP_USE_PACKAGED` - Use system packages (Linux)
- `BUILD_SHARED_LIBS` - Build shared libraries (usually OFF)

### Validation Checklist
Before submitting build system changes:
- [ ] Build succeeds on Windows (if possible)
- [ ] Build succeeds on Linux (if possible)
- [ ] Build succeeds on macOS (if possible)
- [ ] No warnings introduced
- [ ] CMake configuration succeeds
- [ ] All existing targets still build
- [ ] No breaking changes to build commands

## Platform-Specific Notes

### Windows
- Uses NuGet for some dependencies
- Visual Studio generators preferred
- Must use x64 Native Tools Command Prompt

### Linux
- Docker environment ensures reproducible builds
- CentOS base for compatibility
- Poetry required for build preparation

### macOS
- Separate instructions in docs/building-mac.md
- May require Xcode and specific SDK versions
