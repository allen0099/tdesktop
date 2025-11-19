# 64Gram Development Instructions

## Project Overview

This is 64Gram, a fork of Telegram Desktop that provides Windows 64-bit builds with enhanced features. The project is built using C++, Qt framework, and CMake build system.

**Key Facts:**
- Based on Telegram Desktop (GPLv3 with OpenSSL exception)
- Supports Windows 7+, Linux 64-bit, and macOS 10.12+
- Built with C++ and Qt framework
- Uses CMake as build system
- Focus on providing 64-bit builds with enhancements

## Technology Stack

- **Languages**: C++, CMake
- **Framework**: Qt (for cross-platform GUI)
- **Build System**: CMake (minimum version 3.25)
- **Platforms**: Windows, Linux, macOS
- **Version Control**: Git with submodules

## Build Instructions

### Prerequisites
Before building, you need:
1. **API Credentials**: Obtain `api_id` and `api_hash` from [Telegram API](https://core.telegram.org/api/obtaining_api_id)
2. Platform-specific tools:
   - **Windows**: Visual Studio 2022, Windows SDK 10.0.26100.0, Python 3.10, Git
   - **Linux**: Docker (see `docs/building-linux.md`)
   - **macOS**: See `docs/building-mac.md`

### Build Process

#### Windows (64-bit)
```bash
# From x64 Native Tools Command Prompt for VS 2022
git clone --recursive https://github.com/TDesktop-x64/tdesktop.git
cd tdesktop
Telegram\build\prepare\win.bat
cd Telegram
configure.bat x64 -D TDESKTOP_API_ID=YOUR_API_ID -D TDESKTOP_API_HASH=YOUR_API_HASH
# Open out\Telegram.sln in Visual Studio 2022 and build
```

#### Linux (using Docker)
```bash
git clone --recursive https://github.com/TDesktop-x64/tdesktop.git
cd tdesktop
./Telegram/build/prepare/linux.sh
docker run --rm -it -u $(id -u) -v "$PWD:/usr/src/tdesktop" \
    tdesktop:centos_env \
    /usr/src/tdesktop/Telegram/build/docker/centos_env/build.sh \
    -D TDESKTOP_API_ID=YOUR_API_ID \
    -D TDESKTOP_API_HASH=YOUR_API_HASH
```

### Important Build Notes
- Always use `--recursive` when cloning to initialize submodules
- Build artifacts are in the `out` directory
- Debug builds: Use `-e CONFIG=Debug` for Linux or select Debug configuration in Visual Studio

## Coding Standards

### General Principles
1. **Keep code simple and readable** - No abbreviations like `o` for `opacity`, use full names
2. **Descriptive names**: Use `myFunctionThatDoesThings()` instead of `mftdt()`
3. **No whitespace-only changes** - Keep whitespace cleanup in separate commits
4. **Atomic commits** - One logical change per commit
5. **Test your changes** - Verify Telegram Desktop still works properly

### Code Style
- Follow existing code style in the files you modify
- Don't add comments unless they match existing style or explain complex logic
- Use existing libraries when possible
- Only add new dependencies if absolutely necessary

### What NOT to Change
- **NO new features** - Feature decisions are made by Telegram team
- **NO translations** - Use [Telegram's translation platform](https://translations.telegram.org)
- **NO UI/UX changes** - Design decisions come from Telegram team
- **NO breaking changes** - Maintain backward compatibility

## Contribution Guidelines

### Accepted Contributions
✅ **Accepted:**
- Bug fixes
- Code optimization
- Performance improvements
- Documentation fixes
- Build system improvements

❌ **Not Accepted:**
- New features (unless from Telegram team roadmap)
- Translations (use official translation platform)
- UI/UX changes
- New user interface elements

### Pull Request Process
1. **Keep PRs focused** - One issue per PR
2. **Squash commits** - One commit per PR: `git reset --soft HEAD~N && git commit`
3. **No mixed changes** - Don't fix unrelated issues in the same PR
4. **Test thoroughly** - Ensure no crashes or regressions
5. **Good commit messages** - Explain why you make changes, reference issues with `Fix #XXX`

### Before Submitting
- [ ] Build succeeds on target platform(s)
- [ ] Application runs without crashes
- [ ] Changes don't break existing functionality
- [ ] Commit message is clear and descriptive
- [ ] PR is focused on a single issue
- [ ] No unrelated whitespace changes

## File Organization

### Key Directories
- `Telegram/SourceFiles/` - Main application source code
- `Telegram/ThirdParty/` - Third-party libraries
- `cmake/` - CMake helper scripts
- `docs/` - Build and API documentation
- `lib/` - Library submodules
- `out/` - Build output (not committed)

### Important Files
- `CMakeLists.txt` - Root build configuration
- `Telegram/build/version` - Version information
- `CONTRIBUTING.md` - Contribution guidelines
- `README.md` - Project overview
- `features.md` - List of 64Gram-specific features

## Testing

- Manually test changes by building and running the application
- Verify no crashes occur
- Test affected features thoroughly
- Check that existing functionality still works

## Upstream Synchronization

This project stays synchronized with upstream Telegram Desktop:
```bash
git remote add upstream https://github.com/telegramdesktop/tdesktop.git
git fetch upstream master
git log upstream/master  # Review changes
git rebase upstream/master
git push --force
```

## Additional Resources

- **Official Telegram Channel**: https://t.me/tg_x64
- **Discussion Group**: https://t.me/tg_x64_chat
- **Build Documentation**: See `docs/building-*.md` files
- **API Credentials**: https://core.telegram.org/api/obtaining_api_id
- **Features List**: See `features.md`

## Common Commands

```bash
# Build (after configuration)
cmake --build out --config Release

# Clean build
rm -rf out
# Then reconfigure and build

# Check git status
git status
git diff

# View commit history
git log --oneline -20
```

## Security Considerations

- Never commit API credentials to the repository
- Keep sensitive data out of source code
- Follow security best practices for C++ (avoid buffer overflows, use safe string handling)
- Use smart pointers where appropriate
- Validate all user inputs

## Localization

For translation contributions, use the [official Localization Repo](https://github.com/TDesktop-x64/Localization):
- Find your language ID in Telegram's log.txt
- Use the [translation template](https://github.com/TDesktop-x64/Localization/blob/master/en.json)
- Submit translations as Pull Requests to the Localization repo

## Platform-Specific Notes

### Windows
- Use Visual Studio 2022 with Windows SDK 10.0.26100.0
- Run commands from "x64 Native Tools Command Prompt for VS 2022"
- Qt Visual Studio Tools extension recommended for debugging

### Linux
- Docker-based build is recommended
- Poetry required for build scripts
- CentOS environment used in Docker container

### macOS
- See `docs/building-mac.md` for detailed instructions
- Requires macOS 10.12 or higher
