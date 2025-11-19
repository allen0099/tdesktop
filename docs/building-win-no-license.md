# Build instructions for Windows without any Microsoft licensing

This guide shows how to build 64Gram on Windows **without requiring any Microsoft licenses**, including Visual Studio or Build Tools. All methods described here use completely free and open-source tools.

- [Option 1: Using WSL2 (Recommended)](#option-1-using-wsl2-recommended)
- [Option 2: Using Docker Desktop](#option-2-using-docker-desktop)
- [Option 3: Cross-compilation from Linux](#option-3-cross-compilation-from-linux)

## Important Note about Licensing

While **Build Tools for Visual Studio 2022** can be downloaded for free, Microsoft's licensing terms may restrict its use in enterprise environments. The methods in this guide use **no Microsoft tools whatsoever**, making them completely free for any use case.

## Option 1: Using WSL2 (Recommended)

WSL2 (Windows Subsystem for Linux 2) allows you to run a full Linux environment on Windows. This is the **recommended** approach as it provides the best performance and integration with Windows.

### Requirements

- Windows 10 version 2004 or higher, or Windows 11
- Administrator access to install WSL2

### Step 1: Install WSL2

1. Open PowerShell or Windows Command Prompt as **Administrator** and run:

   ```powershell
   wsl --install
   ```

2. Restart your computer when prompted

3. After restart, WSL2 will complete the installation and ask you to create a Linux user account

For detailed instructions, see [Microsoft's WSL installation guide](https://learn.microsoft.com/en-us/windows/wsl/install).

### Step 2: Install Ubuntu (or your preferred distro)

Ubuntu is installed by default with WSL2. To use a different distribution:

```powershell
wsl --list --online
wsl --install -d Ubuntu-22.04
```

### Step 3: Set up build environment in WSL2

Open your WSL2 terminal (search for "Ubuntu" in Windows Start menu) and run:

```bash
# Update package list
sudo apt update && sudo apt upgrade -y

# Install required tools
sudo apt install -y git build-essential cmake python3 python3-pip

# Install poetry for dependency management
curl -sSL https://install.python-poetry.org | python3 -
```

### Step 4: Clone and prepare the repository

```bash
# Create build directory
mkdir -p ~/TBuild
cd ~/TBuild

# Clone repository
git clone --recursive https://github.com/TDesktop-x64/tdesktop.git
cd tdesktop

# Prepare libraries using Docker
./Telegram/build/prepare/linux.sh
```

### Step 5: Obtain API credentials

You will require **api_id** and **api_hash** to access the Telegram API servers. To learn how to obtain them, see [api_credentials.md](api_credentials.md).

### Step 6: Build using Docker

```bash
# Build the project (replace YOUR_API_ID and YOUR_API_HASH)
docker run --rm -it \
    -u $(id -u) \
    -v "$PWD:/usr/src/tdesktop" \
    tdesktop:centos_env \
    /usr/src/tdesktop/Telegram/build/docker/centos_env/build.sh \
    -D TDESKTOP_API_ID=YOUR_API_ID \
    -D TDESKTOP_API_HASH=YOUR_API_HASH
```

For a debug build:

```bash
docker run --rm -it \
    -u $(id -u) \
    -v "$PWD:/usr/src/tdesktop" \
    -e CONFIG=Debug \
    tdesktop:centos_env \
    /usr/src/tdesktop/Telegram/build/docker/centos_env/build.sh \
    -D TDESKTOP_API_ID=YOUR_API_ID \
    -D TDESKTOP_API_HASH=YOUR_API_HASH
```

### Step 7: Access the built files

The built files will be in the `out` directory inside WSL2. You can access them from Windows at:

```
\\wsl$\Ubuntu\home\YOUR_USERNAME\TBuild\tdesktop\out
```

Or copy them to Windows:

```bash
cp -r out /mnt/c/Users/YOUR_WINDOWS_USERNAME/Desktop/telegram-build
```

### Advantages of WSL2 approach

✅ **100% free and open source** - No Microsoft licensing concerns
✅ **Native Linux performance** - Near-native speed with WSL2
✅ **Full Linux toolchain** - Access to all Linux development tools
✅ **Easy file sharing** - Seamless integration with Windows filesystem
✅ **Official support** - WSL2 is officially supported by Microsoft for development

## Option 2: Using Docker Desktop

If you prefer to build without WSL2, you can use Docker Desktop for Windows directly.

### Step 1: Install Docker Desktop

1. Download **Docker Desktop for Windows** from [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. Install and restart your computer
3. Start Docker Desktop

Note: Docker Desktop is free for personal use, education, and small businesses (fewer than 250 employees AND less than $10 million in annual revenue).

### Step 2: Clone the repository

Open PowerShell or Command Prompt:

```powershell
# Create build directory
mkdir C:\TBuild
cd C:\TBuild

# Clone repository (you need Git for Windows installed)
git clone --recursive https://github.com/TDesktop-x64/tdesktop.git
cd tdesktop
```

### Step 3: Prepare libraries

```powershell
# This requires Python installed
cd Telegram\build\prepare
python prepare.py --linux
cd ..\..\..
```

### Step 4: Build with Docker

```powershell
# Build (replace YOUR_API_ID and YOUR_API_HASH)
docker run --rm -it -v "${PWD}:/usr/src/tdesktop" tdesktop:centos_env /usr/src/tdesktop/Telegram/build/docker/centos_env/build.sh -D TDESKTOP_API_ID=YOUR_API_ID -D TDESKTOP_API_HASH=YOUR_API_HASH
```

The built files will be in the `out` directory.

## Option 3: Cross-compilation from Linux

If you have access to a Linux machine (physical or virtual), you can build there and transfer the binaries to Windows.

### On your Linux machine:

Follow the instructions in [building-linux.md](building-linux.md).

### Transfer to Windows:

Use SCP, SFTP, or any file transfer method to copy the built executable to your Windows machine.

## Comparison of Methods

| Method | Pros | Cons | Difficulty |
|--------|------|------|------------|
| **WSL2** | ✅ Best performance<br>✅ Easy file access<br>✅ Native Windows integration | ⚠️ Requires Windows 10/11<br>⚠️ Needs admin rights to install | Easy |
| **Docker Desktop** | ✅ Works on older Windows<br>✅ Isolated environment | ⚠️ Slower file I/O<br>⚠️ Larger disk usage | Medium |
| **Linux VM** | ✅ Complete isolation<br>✅ Full Linux environment | ❌ Requires VM software<br>❌ Higher resource usage | Medium |
| **Remote Linux** | ✅ Use powerful remote server<br>✅ No local resources | ❌ Needs network access<br>❌ File transfer required | Hard |

## Building Windows binaries on Linux

**Important**: The methods above build the **Linux version** of Telegram. If you specifically need Windows `.exe` binaries without using any Microsoft tools, you would need to:

1. Use a cross-compiler like MinGW-w64 on Linux (requires significant CMake changes - not officially supported)
2. Use Wine to run Windows binaries built with the above methods (not recommended for production)
3. Accept that Windows native builds currently require MSVC toolchain

For most users, running the **Linux version through WSL2** provides the best experience without any licensing concerns.

## Alternative: MinGW-w64 (Experimental - Not Officially Supported)

MinGW-w64 is a completely free GCC-based compiler for Windows, but **64Gram does not officially support it**. Attempting to build with MinGW-w64 would require:

- Extensive CMakeLists.txt modifications
- Resolving MSVC-specific code dependencies
- Different library builds for MinGW compatibility
- Significant testing and debugging

This is **not recommended** unless you're willing to contribute significant development effort to port the codebase.

## Troubleshooting

### WSL2: "Cannot connect to Docker daemon"

Make sure Docker is running inside WSL2:

```bash
sudo service docker start
```

Or install Docker Engine directly in WSL2 following [Docker's official guide](https://docs.docker.com/engine/install/ubuntu/).

### Docker Desktop: "Drive sharing" errors

Enable drive sharing in Docker Desktop settings (Settings → Resources → File Sharing).

### Build errors

If you encounter build errors, ensure:
- You're using the latest version of the repository
- All submodules are properly initialized (`git submodule update --init --recursive`)
- Your API credentials are correct

## Summary

For Windows users who want to avoid **any** Microsoft licensing:

1. **Best option**: Use WSL2 (Option 1) - Free, fast, and well-integrated
2. **Alternative**: Use Docker Desktop (Option 2) - Good for older Windows versions
3. **Expert option**: Cross-compile from Linux (Option 3) - Most control

All these methods use **100% free and open-source tools** with no licensing restrictions.
