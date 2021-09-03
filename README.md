# well... looks like taketwo have very dumb assholes.
# 

# re3
| Platform | Debug | Release |
|------------------|-------------|-------------|
| Linux x86 | [![Download](https://api.bintray.com/packages/gtamodding/re3/Debug_win-x86-librw_d3d9-mss/images/download.svg)](https://disk.yandex.ru/d/W9mrpdCR9KZA8Q) | not available |
| Linux x86_64 | [![Download](https://api.bintray.com/packages/gtamodding/re3/Debug_win-x86-librw_gl3_glfw-mss/images/download.svg)](https://disk.yandex.ru/d/DU2sfB0tOOa8Hw) | not available |

What about windows version? Fuck windows.

## Intro

The aim of this project is to reverse GTA III for PC by replacing
parts of the game [one by one](https://en.wikipedia.org/wiki/Ship_of_Theseus)
such that we have a working game at all times.

## How can I try it?

- re3 requires game assets to work, so you **must** own [a copy of GTA III](https://store.steampowered.com/app/12100/Grand_Theft_Auto_III/).
- Build re3 or download it from one of the above links (Debug or Release).
- (Optional) If you want to use optional features like Russian language or menu map, copy the files in /gamefiles folder to your game root folder.
- Move re3.exe to GTA 3 directory and run it.

## Building from Source  

When using premake, you may want to point GTA_VC_RE_DIR environment variable to GTA Vice City root folder if you want the executable to be moved there via post-build script.

Clone the repository with `git clone --recursive https://github.com/ploff/re3.git`. Then `cd re3` into the cloned repository.

<details><summary>Linux Premake</summary>

You need libraries and headers of;

    openal / libopenal-dev
    GLEW / libglew-dev (i386 unavailable on apt after Ubuntu 19.10)
    glfw / libglfw3-dev (min. 3.3 is required, i386 unavailable on apt after Ubuntu 19.10)
    libsndfile1-dev (Caution: install this before libmpg123-dev, optional)
    libmpg123-dev

and compilers set up, i.e. for Ubuntu you should install build-essential.

## Steps

* Run `$ git clone --recursive https://github.com/ploff/re3.git` to clone the project to your PC

Enter the newly created re3 directory;

    If you're on x86/x86_64, run $ ./premake5Linux --with-librw gmake2.
    If you're on i.e. arm/arm64, you need to build your own premake5 from source. Then you can proceed to running premake5 with --with-librw gmake2 arguments.

Enter the build directory and run $ make help to see a help message with supported build configurations and architectures. As of now, refer to one of the available configurations:

    debug_linux-x86-librw_gl3_glfw-oal
    debug_linux-amd64-librw_gl3_glfw-oal
    debug_linux-arm-librw_gl3_glfw-oal
    debug_linux-arm64-librw_gl3_glfw-oal
    release_linux-x86-librw_gl3_glfw-oal
    release_linux-amd64-librw_gl3_glfw-oal
    release_linux-arm-librw_gl3_glfw-oal
    release_linux-arm64-librw_gl3_glfw-oal

* Compile librw and re3 by running $ make config=(your configuration goes here). If you want you can change compiler with setting CC and CXX.

* In case you didn't copy some of the required gamefiles, copy all content from re3/gamefiles/ to the game root directory

(If you didn't set GTA_III_RE_DIR env. variable) Revisit the re3 directory, go to bin and find the appropriate re3 binary you just compiled, and copy it to your game folder with GTA3 inside

* Play the game by running $ ./re3

You can expect poor performance if you're not using Nvidia/AMD drivers. (At least it was in my case with my iGPU)

Remember that re3 is an actively developed project which doesn't have a final version, so in case you'd like to get the game updated you have to reproduce the steps above.

Struct sizes are different then on Windows, so don't enable validating struct sizes.
Needlesly to say, your previous savegames will be incompatible.

</details>

<details><summary>Linux Conan</summary>

Install python and conan, and then run build.
```
conan export vendor/librw librw/master@
mkdir build
cd build
conan install .. reVC/master@ -if build -o reVC:audio=openal -o librw:platform=gl3 -o librw:gl3_gfxlib=glfw --build missing -s reVC:build_type=RelWithDebInfo -s librw:build_type=RelWithDebInfo
conan build .. -if build -bf build -pf package
```
</details>

<details><summary>FreeBSD</summary>

* Get the required packages by either building them in the `/usr/ports` directory or installing them via pkg:

`$ pkg install git premake5 gmake glfw glew openal-soft mpg123 libsndfile libsysinfo`

* Disclaimer: you need to get premake5, there are also other premake versions available, only premake5 is supported. You will also need GNU make and not the built-in BSD make. (Library libsndfile is optional.)

* Run `$ git clone --recursive https://github.com/GTAmodding/re3.git to clone the project to your PC`

* Enter the newly created `re3` directory and run `$ premake5 --with-librw gmake2`, remember to use premake5 you either have built in `/usr/ports` or installed it via `pkg`

* Enter the `build` directory and run `$ gmake help` to see a help message with supported build configurations. As of now, refer to one of the available configurations:

    `debug_bsd-x86-librw_gl3_glfw-oal`
    `debug_bsd-amd64-librw_gl3_glfw-oal`
    `debug_bsd-arm-librw_gl3_glfw-oal`
    `debug_bsd-arm64-librw_gl3_glfw-oal`
    `release_bsd-x86-librw_gl3_glfw-oal`
    `release_bsd-arm-librw_gl3_glfw-oal`
    `release_bsd-arm64-librw_gl3_glfw-oal`

* Compile librw and re3 by running `$ gmake CC=clang CXX=clang++ config=(your configuration goes here)`

* In case you didn't copy some of the required gamefiles, copy all content from re3/gamefiles/ to the game root directory

* (If you didn't set GTA_III_RE_DIR env. variable) Revisit the `re3` directory, go to `bin` and find the appropriate `re3` binary you just compiled, and copy it to your game folder with GTA3 inside

* Play the game by running `$ ./re3`


</details>

<details><summary>Windows</summary>

Assuming you have Visual Studio 2015/2017/2019:
- Run one of the `premake-vsXXXX.cmd` variants on root folder.
- Open build/reVC.sln with Visual Studio and compile the solution.
    
Microsoft recently discontinued its downloads of the DX9 SDK. You can download an archived version here: https://archive.org/details/dxsdk_jun10

**If you choose OpenAL on Windows** You must read [Running OpenAL build on Windows](https://github.com/GTAmodding/re3/wiki/Running-OpenAL-build-on-Windows).
</details>

> :information_source: premake has an `--lto` option if you want the project to be compiled with Link Time Optimization.

> :information_source: There are various settings in [config.h](https://github.com/GTAmodding/re3/tree/miami/src/core/config.h), you may want to take a look there.

> :information_source: reVC uses completely homebrew RenderWare-replacement rendering engine; [librw](https://github.com/aap/librw/). librw comes as submodule of re3, but you also can use LIBRW enviorenment variable to specify path to your own librw.

If you feel the need, you can also use CodeWarrior 7 to compile reVC using the supplied codewarrior/reVC.mcp project - this requires the original RW34 libraries, and the DX8 SDK. The build is unstable compared to the MSVC builds though, and is mostly meant to serve as a reference.

> :information_source: **If you choose OpenAL on Windows** You must read [Running OpenAL build on Windows](https://github.com/GTAmodding/re3/wiki/Running-OpenAL-build-on-Windows).

> :information_source: **Did you notice librw?** re3 uses completely homebrew RenderWare-replacement rendering engine; [librw](https://github.com/aap/librw/). librw comes as submodule of re3, but you also can use LIBRW enviorenment variable to specify path to your own librw.

## Contributing
Please read the [Coding Style](https://github.com/GTAmodding/re3/blob/master/CODING_STYLE.md) Document

### Unreversed / incomplete classes (at least the ones we know)
The following classes have only unused or practically unused code left:
```
CMemoryHeap - only on PS2
NameGrid.cpp - only on mobile (a player name grid, either a very early player name code ala GTA1 or a multiplayer leftover)
PedDebug.cpp - only on mobile (debug code)
HandlingMgr.cpp - debug functions from mobile
CVehicle::ProcessBikeWheel - early bike code (only on mobile)
CAutomobile::DebugCode - debug function from mobile
CBoat::DebugCode - debug function from mobile
CBoat::ModifyHandlingValue - debug function from mobile
CBoat::DisplayHandlingData - debug function from mobile
TexturePools - only on PC (slight RW modification that we don't actually need)
```

