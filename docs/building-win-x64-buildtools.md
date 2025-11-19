# Build instructions for Windows 64-bit using Build Tools (without Visual Studio IDE)

This guide shows how to build 64Gram on Windows without installing Visual Studio 2022 IDE, using only the free **Build Tools for Visual Studio 2022**. This approach avoids any licensing concerns.

- [Prepare folder](#prepare-folder)
- [Install third party software](#install-third-party-software)
- [Clone source code and prepare libraries](#clone-source-code-and-prepare-libraries)
- [Build the project](#build-the-project)

## Prepare folder

The build is done using **Build Tools for Visual Studio 2022** (free, no license required) with **10.0.26100.0** SDK version.

Choose an empty folder for the future build, for example **D:\\TBuild**. It will be named ***BuildPath*** in the rest of this document. Create two folders there, ***BuildPath*\\ThirdParty** and ***BuildPath*\\Libraries**.

All commands (if not stated otherwise) will be launched from **x64 Native Tools Command Prompt for VS 2022** (available after installing Build Tools). Pay attention not to use any other Command Prompt.

### Obtain your API credentials

You will require **api_id** and **api_hash** to access the Telegram API servers. To learn how to obtain them [click here][api_credentials].

## Install third party software

### Build Tools for Visual Studio 2022

Instead of the full Visual Studio IDE, you only need the Build Tools:

1. Download **Build Tools for Visual Studio 2022** from [https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)
   - Scroll down to "All Downloads" → "Tools for Visual Studio" → "Build Tools for Visual Studio 2022"
   - This is **completely free** and does not require any license

2. Run the installer and select the following workloads:
   - **Desktop development with C++**
   
3. In the Installation details panel on the right, ensure these are selected:
   - **MSVC v143 - VS 2022 C++ x64/x86 build tools** (Latest)
   - **Windows 10 SDK (10.0.26100.0)** or the latest version
   - **C++ CMake tools for Windows**
   - **C++ ATL for latest v143 build tools (x86 & x64)**

4. Click Install (this will take some time)

5. After installation, you can find **x64 Native Tools Command Prompt for VS 2022** in your Start Menu under "Visual Studio 2022" folder

### CMake

CMake is included with the Build Tools installation above, but you can also install it separately:

* Download **CMake** from [https://cmake.org/download/](https://cmake.org/download/) (optional if already installed with Build Tools)
  - Choose "Windows x64 Installer"
  - During installation, select "Add CMake to the system PATH for all users"

### Python

* Download **Python 3.10** or later from [https://www.python.org/downloads/](https://www.python.org/downloads/)
  - During installation, check "Add Python to PATH"

### Git

* Download **Git** installer from [https://git-scm.com/download/win](https://git-scm.com/download/win)
  - Use default options during installation

## Clone source code and prepare libraries

Open **x64 Native Tools Command Prompt for VS 2022** (from Start Menu → Visual Studio 2022 → x64 Native Tools Command Prompt for VS 2022), go to ***BuildPath*** and run:

    git clone --recursive https://github.com/TDesktop-x64/tdesktop.git
    tdesktop\Telegram\build\prepare\win.bat

This will download and prepare all necessary third-party libraries.

## Build the project

### Configure the project with CMake

Go to ***BuildPath*\\tdesktop\\Telegram** and run (using [your **api_id** and **api_hash**](#obtain-your-api-credentials)):

    configure.bat x64 -D TDESKTOP_API_ID=YOUR_API_ID -D TDESKTOP_API_HASH=YOUR_API_HASH

This will run CMake and generate Visual Studio solution files in the `out` directory.

### Build using MSBuild (Command Line)

Instead of opening Visual Studio IDE, you can build directly from the command line using MSBuild:

#### Build Debug version:

    msbuild ..\out\Telegram.sln /p:Configuration=Debug /p:Platform=x64

#### Build Release version:

    msbuild ..\out\Telegram.sln /p:Configuration=Release /p:Platform=x64

Alternatively, you can build just the Telegram project instead of the entire solution:

    msbuild ..\out\Telegram.vcxproj /p:Configuration=Release /p:Platform=x64

### Build options

You can speed up the build by using parallel builds:

    msbuild ..\out\Telegram.sln /p:Configuration=Release /p:Platform=x64 /m

The `/m` flag enables parallel building using multiple CPU cores.

### Output location

After a successful build, the executable will be located at:

- **Debug build**: ***BuildPath*\\tdesktop\\out\\Debug\\Telegram.exe**
- **Release build**: ***BuildPath*\\tdesktop\\out\\Release\\Telegram.exe**

## Troubleshooting

### "msbuild is not recognized"

Make sure you're running commands from **x64 Native Tools Command Prompt for VS 2022**, not from a regular Command Prompt or PowerShell window.

### Build errors related to missing SDK

Ensure you have installed Windows SDK 10.0.26100.0 or later. You can verify installed SDKs by checking:

    dir "C:\Program Files (x86)\Windows Kits\10\Include"

### CMake not found

If CMake is not found even after installing Build Tools, you can install it separately from [https://cmake.org/download/](https://cmake.org/download/).

## Comparison with Visual Studio IDE

This build method using Build Tools offers:

**Advantages:**
- ✅ **No license required** - Build Tools are completely free
- ✅ Smaller download and installation size
- ✅ No unnecessary IDE components
- ✅ Perfect for CI/CD and automated builds
- ✅ Same build quality and performance

**Limitations:**
- ❌ No graphical IDE for debugging (you can use VS Code or other editors)
- ❌ No integrated debugger GUI (can use command-line debuggers like WinDbg or VS Code)
- ❌ No Qt Visual Studio Tools integration

For development with debugging support, you may want to use Visual Studio Code with C++ extensions, or install the free Visual Studio Community edition (which has license restrictions for enterprise use).

## Alternative: Using Ninja build system

For even faster builds, you can use Ninja instead of MSBuild:

1. Install Ninja:
   ```
   choco install ninja
   ```
   Or download from [https://github.com/ninja-build/ninja/releases](https://github.com/ninja-build/ninja/releases)

2. Configure with Ninja:
   ```
   configure.bat x64 -D TDESKTOP_API_ID=YOUR_API_ID -D TDESKTOP_API_HASH=YOUR_API_HASH -G Ninja
   ```

3. Build with Ninja:
   ```
   cd ..\out
   ninja
   ```

[api_credentials]: api_credentials.md
