Test
[![Build Windows](https://github.com/Silverlan/pragma/actions/workflows/pragma-windows-ci.yml/badge.svg?branch=master)](https://github.com/Silverlan/pragma/actions/workflows/pragma-windows-ci.yml) [![Build Linux](https://github.com/Silverlan/pragma/actions/workflows/pragma-linux-ci.yml/badge.svg?branch=master)](https://github.com/Silverlan/pragma/actions/workflows/pragma-linux-ci.yml)

<img align="right" width="150" height="150" alt="Pragma Logo" title="Pragma Logo" src="https://wiki.pragma-engine.com/uploads/images/gallery/2022-09/scaled-1680-/pragma-logo-256.png">

What is this?
------
This is the repository for the Pragma Game Engine. For more information, visit the official website: https://pragma-engine.com/


Media
------
<p float="left">
  <img src="https://wiki.pragma-engine.com/uploads/images/gallery/2022-11/scaled-1680-/pragma-2022-11-21-16-31-04.png" width="380" />
  <img src="https://wiki.pragma-engine.com/uploads/images/gallery/2022-11/scaled-1680-/pragma-2022-11-21-16-35-19.png" width="444" /> 
</p>

What platforms and hardware does it work on?
------
- Windows 10 (or newer) / Ubuntu 22.04 LTS (or newer)
- Graphics card: GTX 1050 Ti or newer
- Memory: 4 GiB RAM or more

Build Requirements
------
- ~50 GiB of disk space
- CMake 3.21.4 or newer
- Python 3.9.5 or newer

###### Windows
- Visual Studio 2022 or newer

###### Linux
- clang-14 or newer (Pragma is *not* compatible with gcc!)

Build Instructions
------
Launch a command-line interface and clone the pragma repository (with submodules) into a directory of your choice:
```console
git clone https://github.com/Silverlan/pragma.git --recurse-submodules
```
After that you can simply navigate to the pragma directory and run the python build-script, which will automatically download all dependencies, configure CMake, and build and install the project (this will take several hours):
```console
python build_scripts/build.py --with-pfm --with-all-pfm-modules --with-vr
```
If you don't need the filmmaker, you can omit the `--with-pfm --with-all-pfm-modules` arguments, which will significantly reduce the build time and the required amount of disk space.

###### Linux
> :warning: Before running the build script, you will have to install the following packages:
```console
# Required for the build script
sudo apt-get install python3

# Required for Pragma core
sudo apt install build-essential
sudo add-apt-repository ppa:savoury1/llvm-defaults-14
sudo apt update
sudo apt install clang-14
sudo apt install libstdc++-12-dev
sudo apt install libstdc++6
sudo apt-get install patchelf

# Required for Vulkan
sudo apt-get -qq install -y libwayland-dev libxrandr-dev

sudo apt-get install libxcb-keysyms1-dev
sudo apt-get install xcb libxcb-xkb-dev x11-xkb-utils libx11-xcb-dev libxkbcommon-x11-dev

# Required for GLFW
sudo apt install xorg-dev

# Required for OIDN
sudo apt install git-lfs

# Required for Cycles
sudo apt-get install subversion

# Required for Curl
sudo apt-get install libssl-dev
sudo apt install libssh2-1

# Required for OIIO
sudo apt-get install python3-distutils
```

<br/>
<br/>

Once the build script has been completed, you should find the build files in `pragma/build`, and the install files in `pragma/build/install`. The `install` directory should contain everything you need to run Pragma.

If you make any code changes to the core engine code, you can build the `pragma-install` target to build them. This will also re-install the binaries.

If you make any code changes to a module, you will have to build the module build target first, and then build `pragma-install` afterwards.

### Build Customization

Running the build-script with the arguments above will build and install Pragma and the Pragma Filmmaker with all dependencies. Alternatively you can also configure the build to your liking with the following parameters:

| Parameter                               | Description                                                                          | Default          |
| --------------------------------------- | ------------------------------------------------------------------------------------ | ---------------- |
| `--help`                                | Display this help                                                                    |                  |
| `--generator <generator>`               | The generator to use.                                                                | Windows: `Visual Studio 17 2022`<br/>Linux: `Unix Makefiles` |
| `--c-compiler`                          | [Linux only] The C-compiler to use.                                                  | `clang-14`       |
| `--cxx-compiler`                        | [Linux only] The C++-compiler to use.                                                | `clang++-14`     |
| `--with-essential-client-modules <1/0>` | Include essential modules required to run Pragma.                                    | `1`              |
| `--with-common-modules <1/0>`           | Include non-essential but commonly used modules (e.g. audio and physics modules).    | `1`              |
| `--with-pfm <1/0>`                      | Include the Pragma Filmmaker.                                                        | `0`              |
| `--with-core-pfm-modules <1/0>`         | Include essential PFM modules.                                                       | `1`              |
| `--with-all-pfm-modules <1/0>`          | Include non-essential PFM modules (e.g. chromium and cycles).                        | `0`              |
| `--with-vr <1/0>`                       | Include Virtual Reality support.                                                     | `0`              |
| `--build <1/0>`                         | Build Pragma after configurating and generating build files.                         | `1`              |
| `--build-config <config>`               | The build configuration to use.                                                      | `RelWithDebInfo` |
| `--build-directory <path>`              | Directory to write the build files to. Can be relative or absolute.                  | `build`          |
| `--deps-directory <path>`               | Directory to write the dependency files to. Can be relative or absolute.             | `deps`           |
| `--install-directory <path>`            | Installation directory. Can be relative (to build directory) or absolute.            | `install`        |
| `--verbose <1/0>`                       | Print additional debug information.                                                  | `0`              |
| `--module <moduleName>:<gitUrl>`        | Custom modules to install. Use this argument multiple times to use multiple modules. |                  |

Example for using the `--module` parameter:
```console
--module pr_physx:"https://github.com/Silverlan/pr_physx.git"
```

Modules
------
If you want to add functionality to Pragma without changing the core source code, you can do so by creating a custom binary module. The setup for this is very simple and only takes a few minutes. For more information, please check out the [wiki article](https://wiki.pragma-engine.com/books/pragma-engine/page/binary-modules#bkmrk-custom-modules).

There are also various pre-made binary modules available for Pragma, some of which are already included with this repository, and some of which can be found online:
> :warning: Some of these modules have not been maintained and may no longer be compatible with the latest version of Pragma.
- pr_chromium: https://github.com/Silverlan/pr_chromium
- pr_curl: https://github.com/Silverlan/pr_curl
- pr_mysql: https://github.com/Silverlan/pragma/tree/master/modules/pr_mysql
- pr_pcl: https://github.com/Silverlan/pragma/tree/master/modules/pr_pcl
- pr_prosper_opengl: https://github.com/Silverlan/pr_prosper_opengl
- pr_prosper_vulkan: https://github.com/Silverlan/pr_prosper_vulkan
- pr_socket: https://github.com/Silverlan/pragma/tree/master/modules/pr_socket
- pr_video_recorder: https://github.com/Silverlan/pragma/tree/master/modules/pr_video_recorder
- pr_xml: https://github.com/Silverlan/pragma/tree/master/modules/pr_xml
- pr_audio_dummy: https://github.com/Silverlan/pragma/tree/master/modules/pr_audio_dummy
- pr_cycles: https://github.com/Silverlan/pr_cycles
- pr_source: https://github.com/Silverlan/pr_source
- pr_xatlas: https://github.com/Silverlan/pr_xatlas
- pr_openvr: https://github.com/Silverlan/pr_openvr
- pr_truesky: https://github.com/Silverlan/pr_truesky
- pr_opensubdiv: https://github.com/Silverlan/pr_opensubdiv
- pr_sqlite: https://github.com/Silverlan/pr_sqlite
- pr_xnalara: https://github.com/Silverlan/pr_xnalara
- pr_mpv: https://github.com/Silverlan/pr_mpv
- pr_dmx: https://github.com/Silverlan/pr_dmx
- pr_steamworks: https://github.com/Silverlan/pr_steamworks
- pr_gpl: https://github.com/Silverlan/pr_gpl
- pr_steam_networking_sockets: https://github.com/Silverlan/pr_steam_networking_sockets
- pr_physx: https://github.com/Silverlan/pr_physx
- pr_bullet: https://github.com/Silverlan/pr_bullet
- pr_audio_fmod: https://github.com/Silverlan/pr_audio_fmod
- pr_audio_alure: https://github.com/Silverlan/pr_audio_alure

To install a module, simply run the build-script with the following parameter:
```console
--module <moduleName>:<gitUrl>
```

The build script will clone, build and install the module automatically.

Addons
------
In addition to binary modules, Pragma also uses Lua as scripting language, with thousands of available function, class and library bindings. To get more information about the Lua API, check out the [official wiki](https://wiki.pragma-engine.com/books/lua-api).
An example for a Lua-addon is the [Pragma Filmmaker](https://github.com/Silverlan/pfm).
