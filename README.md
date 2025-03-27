# The Book of Dev - Tauri Experimental Project

- Android App
- Web UI (Leptos, Tailwind CSS, daisyUI)
- Camera and microphone permission

## Overview

**Tauri**

_Tauri is a framework for building tiny, fast binaries for all major desktop and mobile platforms._

<https://tauri.app/>

**Leptos**

_Leptos makes it easy to build applications in the most-loved programming language, combining the best paradigms of modern web development with the power of Rust._

<https://leptos.dev/>

**Trunk**

_Trunk is a WASM web application bundler for Rust. Trunk uses a simple, optional-config pattern for building & bundling WASM, JS snippets & other assets (images, css, scss) via a source HTML file._

<https://trunkrs.dev/>

**Tailwind CSS**

_Tailwind CSS is a utility-first CSS framework for rapidly building modern websites without ever leaving your HTML._

<https://tailwindcss.com/>

**daisyUI**

_daisyUI adds component class names to Tailwind CSS so you can make beautiful websites faster than ever._

<https://daisyui.com/>

## Preparation

Ubuntu 24.10 (oracular) x86_64

[DEVELOPMENT](./DEVELOPMENT.md)

## 1. Android App Project

Device:

Samsung Galaxy Note 10 Plus, Android 12, API 31, USB, 2019.

<https://developer.android.com/studio/debug/dev-options>


```sh
# https://tauri.app/guides/prerequisites/#linux

sudo apt update

sudo apt install -y \
--no-install-recommends \
libwebkit2gtk-4.1-dev \
build-essential \
curl \
wget \
file \
libxdo-dev \
libssl-dev \
libayatana-appindicator3-dev \
librsvg2-dev


# https://tauri.app/start/prerequisites/#android

sudo apt install -y \
--no-install-recommends \
openjdk-17-jdk-headless \
openjdk-17-jre-headless \
java-common

sudo update-java-alternatives --set java-1.17.0-openjdk-amd64


java -version

# openjdk version "17.0.14" 2025-01-21
# OpenJDK Runtime Environment (build 17.0.14+7-Ubuntu-124.10)
# OpenJDK 64-Bit Server VM (build 17.0.14+7-Ubuntu-124.10, mixed mode, sharing)


export JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"


# https://developer.android.com/tools
# https://developer.android.com/studio#command-line-tools-only
# https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip
# SHA-256 2d2d50857e4eb553af5a6dc3ad507a17adf43d115264b1afc116f95c92e5e258


curl -sSfLO https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip

echo '2d2d50857e4eb553af5a6dc3ad507a17adf43d115264b1afc116f95c92e5e258 commandlinetools-linux-11076708_latest.zip' \
> checksum

sha256sum -c checksum

# commandlinetools-linux-11076708_latest.zip: OK

# ls -alH commandlinetools-linux-11076708_latest.zip
# -rw-rw-r-- 1 cavani cavani 153607504 Mar 25 09:19 commandlinetools-linux-11076708_latest.zip

unzip commandlinetools-linux-11076708_latest.zip


cmdline-tools/bin/sdkmanager --help

# Usage:
#   sdkmanager [--uninstall] [<common args>] [--package_file=<file>] [<packages>...]
#   sdkmanager --update [<common args>]
#   sdkmanager --list [<common args>]
#   sdkmanager --list_installed [<common args>]
#   sdkmanager --licenses [<common args>]
#   sdkmanager --version
# 
# With --install (optional), installs or updates packages.
#     By default, the listed packages are installed or (if already installed)
#     updated to the latest version.
# With --uninstall, uninstall the listed packages.
# 
#     <package> is a sdk-style path (e.g. "build-tools;23.0.0" or
#              "platforms;android-23").
#     <package-file> is a text file where each line is a sdk-style path
#                    of a package to install or uninstall.
#     Multiple --package_file arguments may be specified in combination
#     with explicit paths.
# 
# With --update, all installed packages are updated to the latest version.
# 
# With --list, all installed and available packages are printed out.
# 
# With --list_installed, all installed packages are printed out.
# 
# With --licenses, show and offer the option to accept licenses for all
#      available packages that have not already been accepted.
# 
# With --version, prints the current version of sdkmanager.
# 
# Common Arguments:
#     --sdk_root=<sdkRootPath>: Use the specified SDK root instead of the SDK
#                               containing this tool
# 
#     --channel=<channelId>: Include packages in channels up to <channelId>.
#                            Common channels are:
#                            0 (Stable), 1 (Beta), 2 (Dev), and 3 (Canary).
# 
#     --include_obsolete: With --list, show obsolete packages in the
#                         package listing. With --update, update obsolete
#                         packages as well as non-obsolete.
# 
#     --newer: With --list, show only new and/or updatable packages.
# 
#     --no_https: Force all connections to use http rather than https.
# 
#     --proxy=<http | socks>: Connect via a proxy of the given type.
# 
#     --proxy_host=<IP or DNS address>: IP or DNS address of the proxy to use.
# 
#     --proxy_port=<port #>: Proxy port to connect to.
# 
#     --verbose: Enable verbose output.
# 
# * If the env var REPO_OS_OVERRIDE is set to "windows",
#   "macosx", or "linux", packages will be downloaded for that OS.


# Android 34 <- Tauri default

export ANDROID_HOME="$HOME/Android/Sdk"
mkdir -p $ANDROID_HOME

cmdline-tools/bin/sdkmanager \
--sdk_home $ANDROID_HOME \
--install \
'build-tools;34.0.0' \
'cmdline-tools;latest' \
'ndk;29.0.13113456' \
platform-tools \
'platforms;android-34' \
emulator \
'system-images;android-34;google_apis;x86_64'


export NDK_HOME="$ANDROID_HOME/ndk/29.0.13113456"
# export PATH="$PATH:$ANDROID_HOME/emulator:$ANDROID_HOME/platform-tools:$ANDROID_HOME/cmdline-tools/latest/bin"
fish_add_path --path $ANDROID_HOME/emulator $ANDROID_HOME/platform-tools $ANDROID_HOME/cmdline-tools/latest/bin


# Onwards use cmdline-tools installed in Android Sdk Home
rm -rf cmdline-tools commandlinetools-linux-11076708_latest.zip checksum


# https://developer.android.com/studio/run/managing-avds


avdmanager --help

# Usage:
#       avdmanager [global options] [action] [action options]
#       Global options:
#   -s --silent     : Silent mode, shows errors only.
#   -v --verbose    : Verbose mode, shows errors, warnings and all messages.
#      --clear-cache: Clear the SDK Manager repository manifest cache.
#   -h --help       : Help on a specific command.
# 
# Valid actions are composed of a verb and an optional direct object:
# -   list              : Lists existing targets or virtual devices.
# -   list avd          : Lists existing Android Virtual Devices.
# -   list target       : Lists existing targets.
# -   list device       : Lists existing devices.
# - create avd          : Creates a new Android Virtual Device.
# -   move avd          : Moves or renames an Android Virtual Device.
# - delete avd          : Deletes an Android Virtual Device


avdmanager --help create avd

# Usage:
#       avdmanager [global options] create avd [action options]
#       Global options:
#   -s --silent     : Silent mode, shows errors only.
#   -v --verbose    : Verbose mode, shows errors, warnings and all messages.
#      --clear-cache: Clear the SDK Manager repository manifest cache.
#   -h --help       : Help on a specific command.
# 
# Action "create avd":
#   Creates a new Android Virtual Device.
# Options:
#   -c --sdcard  : Path to a shared SD card image, or size of a new sdcard for
#                  the new AVD.
#   -g --tag     : The sys-img tag to use for the AVD. The default is to
#                  auto-select if the platform has only one tag for its system
#                  images.
#   -p --path    : Directory where the new AVD will be created.
#   -k --package : Package path of the system image for this AVD (e.g.
#                  'system-images;android-19;google_apis;x86').
#   -n --name    : Name of the new AVD. [required]
#      --skin    : The optional name of a skin to use with this device.
#   -f --force   : Forces creation (overwrites an existing AVD)
#   -b --abi     : The ABI to use for the AVD. The default is to auto-select the
#                  ABI if the platform has only one ABI for its system images.
#   -d --device  : The optional device definition to use. Can be a device index
#                  or id.


avdmanager create avd \
--name Pixel_6_Pro_API_34 \
--device pixel_6_pro \
--package 'system-images;android-34;google_apis;x86_64'


avdmanager list avd

# Available Android Virtual Devices:
#     Name: Pixel_6_Pro_API_34
#   Device: pixel_6_pro (Google)
#     Path: /home/cavani/.config/.android/avd/Pixel_6_Pro_API_34.avd
#   Target: Google APIs (Google Inc.)
#           Based on: Android 14.0 ("UpsideDownCake") Tag/ABI: google_apis/x86_64
#   Sdcard: 512 MB


export ANDROID_AVD_HOME="$HOME/.config/.android/avd"


emulator -list-avds

# Pixel_6_Pro_API_34


emulator -avd Pixel_6_Pro_API_34 -netdelay none -netspeed full

# INFO         | Android emulator version 35.4.9.0 (build_id 13025442) (CL:N/A)
# INFO         | Graphics backend: gfxstream
# INFO         | Found systemPath /home/cavani/Android/Sdk/system-images/android-34/google_apis/x86_64/
# INFO         | Increasing RAM size to 3072MB
# INFO         | Checking system compatibility:
# INFO         |   Checking: hasSufficientDiskSpace
# INFO         |      Ok: Disk space requirements to run avd: `Pixel_6_Pro_API_34` are met
# INFO         |   Checking: hasSufficientHwGpu
# INFO         |      Ok: Hardware GPU requirements to run avd: `Pixel_6_Pro_API_34` are passed
# INFO         |   Checking: hasSufficientSystem
# INFO         |      Ok: System requirements to run avd: `Pixel_6_Pro_API_34` are met
# WARNING      | Failed to process .ini file /home/cavani/.config/.android/avd/../avd/Pixel_6_Pro_API_34.avd/quickbootChoice.ini for reading.
# INFO         | Warning: Could not find the Qt platform plugin "wayland" in "/home/cavani/Android/Sdk/emulator/lib64/qt/plugins" (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_btn_xr_environment_living_room_day_clicked() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_btn_xr_environment_living_room_night_clicked() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_new_posture_requested(int) (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_dismiss_posture_selection_dialog() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_new_resizable_requested(PresetEmulatorSizeType) (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_dismiss_resizable_dialog() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_xr_environment_mode_changed(int) (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_dismiss_xr_environment_mode_dialog() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_xr_input_mode_changed(int) (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_dismiss_xr_input_mode_dialog() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_sleep_timer_done() (:0, )
# INFO         | Warning: QMetaObject::connectSlotsByName: No matching signal for on_unfold_timer_done() (:0, )
# INFO         | Storing crashdata in: /tmp/android-cavani/emu-crash-35.4.9.db, detection is enabled for process: 372410
# INFO         | Guest Driver: Auto (ext controls)
# library_mode host gpu mode host
# INFO         | emuglConfig_get_vulkan_hardware_gpu_support_info: Found physical GPU 'Intel(R) Graphics (RPL-U)', type: VK_PHYSICAL_DEVICE_TYPE_INTEGRATED_GPU, apiVersion: 1.3.289, driverVersion: 24.2.8
# INFO         | emuglConfig_get_vulkan_hardware_gpu_support_info: Found physical GPU 'llvmpipe (LLVM 19.1.1, 256 bits)', type: VK_PHYSICAL_DEVICE_TYPE_CPU, apiVersion: 1.3.289, driverVersion: 0.0.1
# INFO         | emuglConfig_get_vulkan_hardware_gpu_support_info: Found physical GPU 'NVIDIA GeForce MX570 A', type: VK_PHYSICAL_DEVICE_TYPE_DISCRETE_GPU, apiVersion: 1.3.289, driverVersion: 565.57
# INFO         | GPU device local memory = 2048MB
# INFO         | Initializing hardware OpenGLES emulation support
# I0326 10:34:02.175544  372410 opengles.cpp:285] android_startOpenglesRenderer: gpu info
# I0326 10:34:02.175585  372410 opengles.cpp:286]
# INFO         | Not raising nofile soft limit from 1073741816.
# INFO         | HealthMonitor disabled.
# INFO         | SharedLibrary::open for [libvulkan.so]
# INFO         | SharedLibrary::open for [libvulkan.so]: not found in map, open for the first time
# INFO         | SharedLibrary::open for [libvulkan.so] (posix): begin
# INFO         | SharedLibrary::open for [libvulkan.so] (posix,linux): call dlopen on [libvulkan.so]
# INFO         | SharedLibrary::open for [libvulkan.so] failed (posix). dlerror: []
# INFO         | SharedLibrary::open for [libvulkan.so.1]
# INFO         | SharedLibrary::open for [libvulkan.so.1]: not found in map, open for the first time
# INFO         | SharedLibrary::open for [libvulkan.so.1] (posix): begin
# INFO         | SharedLibrary::open for [libvulkan.so.1] (posix,linux): call dlopen on [libvulkan.so.1]
# INFO         | Added library: libvulkan.so.1
# INFO         | Selecting Vulkan device: NVIDIA GeForce MX570 A, Version: 1.3.289
# INFO         | Supports id properties, got a vulkan device UUID
# INFO         | SharedLibrary::open for [libX11]
# INFO         | SharedLibrary::open for [libX11]: not found in map, open for the first time
# INFO         | SharedLibrary::open for [libX11] (posix): begin
# INFO         | SharedLibrary::open for [libX11] (posix,linux): call dlopen on [libX11.so]
# INFO         | SharedLibrary::open for [libGL.so.1]
# INFO         | SharedLibrary::open for [libGL.so.1]: not found in map, open for the first time
# INFO         | SharedLibrary::open for [libGL.so.1] (posix): begin
# INFO         | SharedLibrary::open for [libGL.so.1] (posix,linux): call dlopen on [libGL.so.1]
# INFO         | SharedLibrary::open for [libshadertranslator.so]: not found in map, open for the first time
# INFO         | SharedLibrary::open for [libshadertranslator.so] (posix): begin
# INFO         | SharedLibrary::open for [libshadertranslator.so] (posix,linux): call dlopen on [libshadertranslator.so]
# INFO         | Initializing VkEmulation features:
# INFO         |     glInteropSupported: false
# INFO         |     useDeferredCommands: true
# INFO         |     createResourceWithRequirements: true
# INFO         |     useVulkanComposition: false
# INFO         |     useVulkanNativeSwapchain: false
# INFO         |     enable guestRenderDoc: false
# INFO         |     ASTC LDR emulation mode: 2
# INFO         |     enable ETC2 emulation: true
# INFO         |     enable Ycbcr emulation: false
# INFO         |     guestVulkanOnly: false
# INFO         |     useDedicatedAllocations: false
# INFO         | Graphics Adapter Vendor Google (Intel)
# INFO         | Graphics Adapter Android Emulator OpenGL ES Translator (Mesa Intel(R) Graphics (RPL-U))
# INFO         | Graphics API Version OpenGL ES 3.0 (4.6 (Core Profile) Mesa 24.2.8-1ubuntu1~24.10.1)
# INFO         | Graphics API Extensions GL_OES_EGL_sync GL_OES_EGL_image GL_OES_EGL_image_external GL_OES_depth24 GL_OES_depth32 GL_OES_element_index_uint GL_OES_texture_float GL_OES_texture_float_linear GL_OES_compressed_paletted_texture GL_OES_compressed_ETC1_RGB8_texture GL_OES_depth_texture GL_OES_texture_half_float GL_OES_texture_half_float_linear GL_OES_packed_depth_stencil GL_OES_vertex_half_float GL_OES_texture_npot GL_OES_rgb8_rgba8 GL_EXT_color_buffer_float GL_EXT_color_buffer_half_float GL_EXT_texture_format_BGRA8888 GL_APPLE_texture_format_BGRA8888
# INFO         | Graphics Device Extensions N/A
# INFO         | Sending adb public key [QAAAAHv0A2ZNNfZjk98nVLFxIJV0X5ME9+M8KNMJVSj0zIvWRr98fzs1MHulpfNxLa9BXXnxCi+6HR9JdnJA3RSi9kLiCPh9IowuMgkoRO9ONLDYMtIXfPu1WImPN2c75dcnLEZbUkWUSVz9JACYK1ldDpypbWUioh3DT1ALsUAyUQucFPfxF0M3iK2pqLc3pyxJf0k3NmIJmXDYb7J8hbcKzSBJrt3O8Rjhmg7OfuLkI50jjnbAlIFtvuyfAoZTEiRPjl9Jp9p+3LM4Rg2bYe/boDUUq/2Co3+9jsUZ+RhPICkZggabR9TXXbr7BlvHelcLcnkFWwcfZNNVxZfSWv0u3dpBnhOm9RS49KSXuKuZpFDbvaA6ikRM2s3YBaY8tnuBjwIhOzYq0SjTR1WReggOt5+6izALQTwh9mnb1PXpVqZpqEcp8udDVgB8zr4ulMzBYwhMxwP6Ipza52Kbnu90fU6sVwHPSxi0tOpr29ynqivF1ZufA9Ay4wGS8ki2wu2c7MMjFJdx4HAWvbUYJ4btUH5XhDu/cKKGfM5y2KkTLdmjv5VhzRGQVsphKwr6UE51+wiMmyx2HLlkgc0YvxjAG0gC7tNlR1a8nBcOYNa9gYHlu/HfDdMA6mpJ/sPimabPTVMLOdMlPHw/nesTerLxKjxxF9CIbir5jI4GmXK3OfRlt2dQGwEAAQA= cavani@unknown]
# INFO         | Userspace boot properties:
# INFO         |   androidboot.boot_devices=pci0000:00/0000:00:03.0 pci0000:00/0000:00:06.0
# INFO         |   androidboot.dalvik.vm.heapsize=576m
# INFO         |   androidboot.debug.hwui.renderer=skiagl
# INFO         |   androidboot.hardware=ranchu
# INFO         |   androidboot.hardware.gltransport=pipe
# INFO         |   androidboot.hardware.vulkan=ranchu
# INFO         |   androidboot.logcat=*:V
# INFO         |   androidboot.opengles.version=196609
# INFO         |   androidboot.qemu=1
# INFO         |   androidboot.qemu.adb.pubkey=QAAAAHv0A2ZNNfZjk98nVLFxIJV0X5ME9+M8KNMJVSj0zIvWRr98fzs1MHulpfNxLa9BXXnxCi+6HR9JdnJA3RSi9kLiCPh9IowuMgkoRO9ONLDYMtIXfPu1WImPN2c75dcnLEZbUkWUSVz9JACYK1ldDpypbWUioh3DT1ALsUAyUQucFPfxF0M3iK2pqLc3pyxJf0k3NmIJmXDYb7J8hbcKzSBJrt3O8Rjhmg7OfuLkI50jjnbAlIFtvuyfAoZTEiRPjl9Jp9p+3LM4Rg2bYe/boDUUq/2Co3+9jsUZ+RhPICkZggabR9TXXbr7BlvHelcLcnkFWwcfZNNVxZfSWv0u3dpBnhOm9RS49KSXuKuZpFDbvaA6ikRM2s3YBaY8tnuBjwIhOzYq0SjTR1WReggOt5+6izALQTwh9mnb1PXpVqZpqEcp8udDVgB8zr4ulMzBYwhMxwP6Ipza52Kbnu90fU6sVwHPSxi0tOpr29ynqivF1ZufA9Ay4wGS8ki2wu2c7MMjFJdx4HAWvbUYJ4btUH5XhDu/cKKGfM5y2KkTLdmjv5VhzRGQVsphKwr6UE51+wiMmyx2HLlkgc0YvxjAG0gC7tNlR1a8nBcOYNa9gYHlu/HfDdMA6mpJ/sPimabPTVMLOdMlPHw/nesTerLxKjxxF9CIbir5jI4GmXK3OfRlt2dQGwEAAQA= cavani@unknown
# INFO         |   androidboot.qemu.avd_name=Pixel_6_Pro_API_34
# INFO         |   androidboot.qemu.camera_hq_edge_processing=0
# INFO         |   androidboot.qemu.camera_protocol_ver=1
# INFO         |   androidboot.qemu.cpuvulkan.version=4202496
# INFO         |   androidboot.qemu.gltransport.drawFlushInterval=800
# INFO         |   androidboot.qemu.gltransport.name=pipe
# INFO         |   androidboot.qemu.hwcodec.avcdec=2
# INFO         |   androidboot.qemu.hwcodec.hevcdec=2
# INFO         |   androidboot.qemu.hwcodec.vpxdec=2
# INFO         |   androidboot.qemu.settings.system.screen_off_timeout=2147483647
# INFO         |   androidboot.qemu.virtiowifi=1
# INFO         |   androidboot.qemu.vsync=60
# INFO         |   androidboot.serialno=EMULATOR35X4X9X0
# INFO         |   androidboot.vbmeta.digest=451e57ed688ff746d2c636d52169ade253e2bc5bb30a5c3ff23679ca15d3bcd3
# INFO         |   androidboot.vbmeta.hash_alg=sha256
# INFO         |   androidboot.vbmeta.size=6656
# INFO         |   androidboot.veritymode=enforcing
# INFO         | Monitoring duration of emulator setup.
# WARNING      | The emulator now requires a signed jwt token for gRPC access! Use the -grpc flag if you really want an open unprotected grpc port
# INFO         | Using security allow list from: /home/cavani/Android/Sdk/emulator/lib/emulator_access.json
# WARNING      | *** Basic token auth should only be used by android-studio ***
# INFO         | The active JSON Web Key Sets can be found here: /run/user/1001/avd/running/372410/jwks/32fa91de-c182-4f33-9adc-d4bbe42bdabe/active.jwk
# INFO         | Scanning /run/user/1001/avd/running/372410/jwks/32fa91de-c182-4f33-9adc-d4bbe42bdabe for jwk keys.
# INFO         | Started GRPC server at 127.0.0.1:8554, security: Local, auth: +token
# INFO         | Advertising in: /run/user/1001/avd/running/pid_372410.ini
# INFO         | Setting display: 0 configuration to: 1440x3120, dpi: 560x560
# INFO         | setDisplayActiveConfig 0
# INFO         | Checking system compatibility:
# INFO         |   Checking: hasSufficientDiskSpace
# INFO         |      Ok: Disk space requirements to run avd: `Pixel_6_Pro_API_34` are met
# INFO         |   Checking: hasSufficientHwGpu
# INFO         |      Ok: Hardware GPU requirements to run avd: `Pixel_6_Pro_API_34` are passed
# INFO         |   Checking: hasSufficientSystem
# INFO         |      Ok: System requirements to run avd: `Pixel_6_Pro_API_34` are met
# INFO         | Loading snapshot 'default_boot'...
# WARNING      | Device 'cache' does not have the requested snapshot 'default_boot'
# WARNING      | Failed to load snapshot 'default_boot'
# INFO         | Boot completed in 40901 ms
# INFO         | Increasing screen off timeout, logcat buffer size to 2M.
# INFO         | Activated packet streamer for bluetooth emulation


# [Other terminal]

echo '# Android env
export JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
export ANDROID_HOME="$HOME/Android/Sdk"
export ANDROID_AVD_HOME="$HOME/.config/.android/avd"
export NDK_HOME="$ANDROID_HOME/ndk/29.0.13113456"
# export PATH="$PATH:$ANDROID_HOME/emulator:$ANDROID_HOME/platform-tools:$ANDROID_HOME/cmdline-tools/latest/bin"
fish_add_path --path $ANDROID_HOME/emulator $ANDROID_HOME/platform-tools $ANDROID_HOME/cmdline-tools/latest/bin' \
> env.sh

source env.sh


# https://developer.android.com/tools/adb


adb --help

# Android Debug Bridge version 1.0.41
# Version 35.0.2-12147458
# Installed as /home/cavani/Android/Sdk/platform-tools/adb
# Running on Linux 6.11.0-19-generic (x86_64)
# 
# global options:
#  -a                       listen on all network interfaces, not just localhost
#  -d                       use USB device (error if multiple devices connected)
#  -e                       use TCP/IP device (error if multiple TCP/IP devices available)
#  -s SERIAL                use device with given serial (overrides $ANDROID_SERIAL)
#  -t ID                    use device with given transport id
#  -H                       name of adb server host [default=localhost]
#  -P                       port of adb server [default=5037]
#  -L SOCKET                listen on given socket for adb server [default=tcp:localhost:5037]
#  --one-device SERIAL|USB  only allowed with 'start-server' or 'server nodaemon', server will only connect to one USB device, specified by a serial number or USB device address.
#  --exit-on-write-error    exit if stdout is closed
# 
# general commands:
#  devices [-l]             list connected devices (-l for long output)
#  help                     show this help message
#  version                  show version num
# 
# networking:
#  connect HOST[:PORT]      connect to a device via TCP/IP [default port=5555]
#  disconnect [HOST[:PORT]]
#      disconnect from given TCP/IP device [default port=5555], or all
#  pair HOST[:PORT] [PAIRING CODE]
#      pair with a device for secure TCP/IP communication
#  forward --list           list all forward socket connections
#  forward [--no-rebind] LOCAL REMOTE
#      forward socket connection using:
#        tcp:<port> (<local> may be "tcp:0" to pick any open port)
#        localabstract:<unix domain socket name>
#        localreserved:<unix domain socket name>
#        localfilesystem:<unix domain socket name>
#        dev:<character device name>
#        dev-raw:<character device name> (open device in raw mode)
#        jdwp:<process pid> (remote only)
#        vsock:<CID>:<port> (remote only)
#        acceptfd:<fd> (listen only)
#  forward --remove LOCAL   remove specific forward socket connection
#  forward --remove-all     remove all forward socket connections
#  reverse --list           list all reverse socket connections from device
#  reverse [--no-rebind] REMOTE LOCAL
#      reverse socket connection using:
#        tcp:<port> (<remote> may be "tcp:0" to pick any open port)
#        localabstract:<unix domain socket name>
#        localreserved:<unix domain socket name>
#        localfilesystem:<unix domain socket name>
#  reverse --remove REMOTE  remove specific reverse socket connection
#  reverse --remove-all     remove all reverse socket connections from device
#  mdns check               check if mdns discovery is available
#  mdns services            list all discovered services
# 
# file transfer:
#  push [--sync] [-z ALGORITHM] [-Z] LOCAL... REMOTE
#      copy local files/directories to device
#      -n: dry run: push files to device without storing to the filesystem
#      -q: suppress progress messages
#      -Z: disable compression
#      -z: enable compression with a specified algorithm (any/none/brotli/lz4/zstd)
#      --sync: only push files that have different timestamps on the host than the device
#  pull [-a] [-z ALGORITHM] [-Z] REMOTE... LOCAL
#      copy files/dirs from device
#      -a: preserve file timestamp and mode
#      -q: suppress progress messages
#      -Z: disable compression
#      -z: enable compression with a specified algorithm (any/none/brotli/lz4/zstd)
#  sync [-l] [-z ALGORITHM] [-Z] [all|data|odm|oem|product|system|system_ext|vendor]
#      sync a local build from $ANDROID_PRODUCT_OUT to the device (default all)
#      -l: list files that would be copied, but don't copy them
#      -n: dry run: push files to device without storing to the filesystem
#      -q: suppress progress messages
#      -Z: disable compression
#      -z: enable compression with a specified algorithm (any/none/brotli/lz4/zstd)
# 
# shell:
#  shell [-e ESCAPE] [-n] [-Tt] [-x] [COMMAND...]
#      run remote shell command (interactive shell if no command given)
#      -e: choose escape character, or "none"; default '~'
#      -n: don't read from stdin
#      -T: disable pty allocation
#      -t: allocate a pty if on a tty (-tt: force pty allocation)
#      -x: disable remote exit codes and stdout/stderr separation
#  emu COMMAND              run emulator console command
# 
# app installation (see also `adb shell cmd package help`):
#  install [-lrtsdg] [--instant] PACKAGE
#      push a single package to the device and install it
#  install-multiple [-lrtsdpg] [--instant] PACKAGE...
#      push multiple APKs to the device for a single package and install them
#  install-multi-package [-lrtsdpg] [--instant] PACKAGE...
#      push one or more packages to the device and install them atomically
#      -r: replace existing application
#      -t: allow test packages
#      -d: allow version code downgrade (debuggable packages only)
#      -p: partial application install (install-multiple only)
#      -g: grant all runtime permissions
#      --abi ABI: override platform's default ABI
#      --instant: cause the app to be installed as an ephemeral install app
#      --no-streaming: always push APK to device and invoke Package Manager as separate steps
#      --streaming: force streaming APK directly into Package Manager
#      --fastdeploy: use fast deploy
#      --no-fastdeploy: prevent use of fast deploy
#      --force-agent: force update of deployment agent when using fast deploy
#      --date-check-agent: update deployment agent when local version is newer and using fast deploy
#      --version-check-agent: update deployment agent when local version has different version code and using fast deploy
#      --local-agent: locate agent files from local source build (instead of SDK location)
#      (See also `adb shell pm help` for more options.)
#  uninstall [-k] PACKAGE
#      remove this app package from the device
#      '-k': keep the data and cache directories
# 
# debugging:
#  bugreport [PATH]
#      write bugreport to given PATH [default=bugreport.zip];
#      if PATH is a directory, the bug report is saved in that directory.
#      devices that don't support zipped bug reports output to stdout.
#  jdwp                     list pids of processes hosting a JDWP transport
#  logcat                   show device log (logcat --help for more)
# 
# security:
#  disable-verity           disable dm-verity checking on userdebug builds
#  enable-verity            re-enable dm-verity checking on userdebug builds
#  keygen FILE
#      generate adb public/private key; private key stored in FILE,
# 
# scripting:
#  wait-for[-TRANSPORT]-STATE...
#      wait for device to be in a given state
#      STATE: device, recovery, rescue, sideload, bootloader, or disconnect
#      TRANSPORT: usb, local, or any [default=any]
#  get-state                print offline | bootloader | device
#  get-serialno             print <serial-number>
#  get-devpath              print <device-path>
#  remount [-R]
#       remount partitions read-write. if a reboot is required, -R will
#       will automatically reboot the device.
#  reboot [bootloader|recovery|sideload|sideload-auto-reboot]
#      reboot the device; defaults to booting system image but
#      supports bootloader and recovery too. sideload reboots
#      into recovery and automatically starts sideload mode,
#      sideload-auto-reboot is the same but reboots after sideloading.
#  sideload OTAPACKAGE      sideload the given full OTA package
#  root                     restart adbd with root permissions
#  unroot                   restart adbd without root permissions
#  usb                      restart adbd listening on USB
#  tcpip PORT               restart adbd listening on TCP on PORT
# 
# internal debugging:
#  start-server             ensure that there is a server running
#  kill-server              kill the server if it is running
#  reconnect                kick connection from host side to force reconnect
#  reconnect device         kick connection from device side to force reconnect
#  reconnect offline        reset offline/unauthorized devices to force reconnect
# 
# usb:
#  attach                   attach a detached USB device
#  detach                   detach from a USB device to allow use by other processes
# environment variables:
#  $ADB_TRACE
#      comma/space separated list of debug info to log:
#      all,adb,sockets,packets,rwx,usb,sync,sysdeps,transport,jdwp,services,auth,fdevent,shell,incremental
#  $ADB_VENDOR_KEYS         colon-separated list of keys (files or directories)
#  $ANDROID_SERIAL          serial number to connect to (see -s)
#  $ANDROID_LOG_TAGS        tags to be used by logcat (see logcat --help)
#  $ADB_LOCAL_TRANSPORT_MAX_PORT max emulator scan port (default 5585, 16 emus)
#  $ADB_MDNS_AUTO_CONNECT   comma-separated list of mdns services to allow auto-connect (default adb-tls-connect)
# 
# Online documentation: https://android.googlesource.com/platform/packages/modules/adb/+/refs/heads/main/docs/user/adb.1.md


adb devices

# * daemon not running; starting now at tcp:5037
# * daemon started successfully
# List of devices attached
# emulator-5554   device


sdkmanager --list_installed

# [=======================================] 100% Fetch remote repository...
# Installed packages:
#   Path                                        | Version           | Description                                | Location
#   -------                                     | -------           | -------                                    | -------
#   build-tools;34.0.0                          | 34.0.0            | Android SDK Build-Tools 34                 | build-tools/34.0.0
#   cmdline-tools;latest                        | 19.0              | Android SDK Command-line Tools (latest)    | cmdline-tools/latest
#   emulator                                    | 35.4.9            | Android Emulator                           | emulator
#   ndk;29.0.13113456                           | 29.0.13113456 rc1 | NDK (Side by side) 29.0.13113456           | ndk/29.0.13113456
#   platform-tools                              | 35.0.2            | Android SDK Platform-Tools                 | platform-tools
#   platforms;android-34                        | 3                 | Android SDK Platform 34                    | platforms/android-34
#   system-images;android-34;google_apis;x86_64 | 14                | Google APIs Intel x86_64 Atom System Image | system-images/android-34/google_apis/x86_64



rustup target add \
aarch64-linux-android \
armv7-linux-androideabi \
i686-linux-android \
x86_64-linux-android \
wasm32-unknown-unknown



cargo binstall -y create-tauri-app tauri-cli trunk

cargo create-tauri-app --help

# cargo create-tauri-app 4.5.9
# Tauri Programme within The Commons Conservancy
# Rapidly scaffold out a new tauri app project.
# 
# USAGE:
#   cargo create-tauri-app [OPTIONS] [PROJECTNAME]
# 
# ARGS:
#   <PROJECTNAME>                 Specify project name which is used for the directory, package.json and Cargo.toml
# 
# OPTIONS:
#   -m, --manager <MANAGER>       Specify preferred package manager [cargo, pnpm, yarn, npm, deno, bun, dotnet]
#   -t, --template <TEMPLATE>     Specify the UI template to use [vanilla, vanilla-ts, vue, vue-ts, svelte, svelte-ts, react, react-ts, solid, solid-ts, yew, leptos, sycamore, angular, preact, preact-ts, blazor, dioxus]
#                     --identifier <identifier> Specify a unique identifier for your application
#   -y, --yes                     Skip prompts and use defaults where applicable
#   -f, --force                   Force create the directory even if it is not empty.
#                     --tauri-version [1 | 2]   Bootstrap a project using the provided Tauri version. Defaults to the latest stable release.
#   -h, --help                    Prints help information
#   -v, --version                 Prints version informatio


cargo create-tauri-app \
--manager cargo \
--template leptos \
--identifier domain.machine.v1 \
--tauri-version 2 \
machine-v1

# Template created! To get started run:
#   cd machine-v1
#   cargo tauri android init
# 
# For Desktop development, run:
#   cargo tauri dev
# 
# For Android development, run:
#   cargo tauri android dev

cd machine-v1

sed -i \
-e 's/^version = "0.1.0"/version = "0.0.1"/1' \
-e 's/^edition = "2021"/edition = "2024"/1' \
Cargo.toml

sed -i \
-e 's/^version = "0.1.0"/version = "0.0.1"/1' \
-e 's/^edition = "2021"/edition = "2024"/1' \
-e '/^authors/d' \
-e '/^description/d' \
src-tauri/Cargo.toml

sed -i 's/"version": "0.1.0"/"version": "0.0.1"/1' src-tauri/tauri.conf.json

# https://tauri.app/reference/config/
# https://tauri.app/reference/config/#androidconfig
# Android Min SDK Version = 31 (Android 12) ~2019

mv src-tauri/tauri{,-orig}.conf.json

jq '.bundle.android.minSdkVersion = 31' src-tauri/tauri-orig.conf.json > src-tauri/tauri.conf.json


cargo tauri android init

# Generating Android Studio project...
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri" relative to "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/machine_v1" is "../../../"
# victory: Project generated successfully!
#     Make cool apps! 🌻 🐕 🎉


find src-tauri/gen/android/

# src-tauri/gen/android/
# src-tauri/gen/android/buildSrc
# src-tauri/gen/android/buildSrc/build.gradle.kts
# src-tauri/gen/android/buildSrc/src
# src-tauri/gen/android/buildSrc/src/main
# src-tauri/gen/android/buildSrc/src/main/java
# src-tauri/gen/android/buildSrc/src/main/java/domain
# src-tauri/gen/android/buildSrc/src/main/java/domain/machine
# src-tauri/gen/android/buildSrc/src/main/java/domain/machine/v1
# src-tauri/gen/android/buildSrc/src/main/java/domain/machine/v1/kotlin
# src-tauri/gen/android/buildSrc/src/main/java/domain/machine/v1/kotlin/BuildTask.kt
# src-tauri/gen/android/buildSrc/src/main/java/domain/machine/v1/kotlin/RustPlugin.kt
# src-tauri/gen/android/build.gradle.kts
# src-tauri/gen/android/gradle
# src-tauri/gen/android/gradle/wrapper
# src-tauri/gen/android/gradle/wrapper/gradle-wrapper.jar
# src-tauri/gen/android/gradle/wrapper/gradle-wrapper.properties
# src-tauri/gen/android/.editorconfig
# src-tauri/gen/android/gradlew.bat
# src-tauri/gen/android/gradle.properties
# src-tauri/gen/android/app
# src-tauri/gen/android/app/proguard-rules.pro
# src-tauri/gen/android/app/build.gradle.kts
# src-tauri/gen/android/app/src
# src-tauri/gen/android/app/src/main
# src-tauri/gen/android/app/src/main/java
# src-tauri/gen/android/app/src/main/java/domain
# src-tauri/gen/android/app/src/main/java/domain/machine
# src-tauri/gen/android/app/src/main/java/domain/machine/v1
# src-tauri/gen/android/app/src/main/java/domain/machine/v1/MainActivity.kt
# src-tauri/gen/android/app/src/main/AndroidManifest.xml
# src-tauri/gen/android/app/src/main/assets
# src-tauri/gen/android/app/src/main/res
# src-tauri/gen/android/app/src/main/res/values-night
# src-tauri/gen/android/app/src/main/res/values-night/themes.xml
# src-tauri/gen/android/app/src/main/res/values
# src-tauri/gen/android/app/src/main/res/values/themes.xml
# src-tauri/gen/android/app/src/main/res/values/strings.xml
# src-tauri/gen/android/app/src/main/res/values/colors.xml
# src-tauri/gen/android/app/src/main/res/mipmap-mdpi
# src-tauri/gen/android/app/src/main/res/mipmap-mdpi/ic_launcher.png
# src-tauri/gen/android/app/src/main/res/mipmap-mdpi/ic_launcher_foreground.png
# src-tauri/gen/android/app/src/main/res/mipmap-mdpi/ic_launcher_round.png
# src-tauri/gen/android/app/src/main/res/layout
# src-tauri/gen/android/app/src/main/res/layout/activity_main.xml
# src-tauri/gen/android/app/src/main/res/mipmap-xxxhdpi
# src-tauri/gen/android/app/src/main/res/mipmap-xxxhdpi/ic_launcher.png
# src-tauri/gen/android/app/src/main/res/mipmap-xxxhdpi/ic_launcher_foreground.png
# src-tauri/gen/android/app/src/main/res/mipmap-xxxhdpi/ic_launcher_round.png
# src-tauri/gen/android/app/src/main/res/mipmap-xxhdpi
# src-tauri/gen/android/app/src/main/res/mipmap-xxhdpi/ic_launcher.png
# src-tauri/gen/android/app/src/main/res/mipmap-xxhdpi/ic_launcher_foreground.png
# src-tauri/gen/android/app/src/main/res/mipmap-xxhdpi/ic_launcher_round.png
# src-tauri/gen/android/app/src/main/res/drawable-v24
# src-tauri/gen/android/app/src/main/res/drawable-v24/ic_launcher_foreground.xml
# src-tauri/gen/android/app/src/main/res/xml
# src-tauri/gen/android/app/src/main/res/xml/file_paths.xml
# src-tauri/gen/android/app/src/main/res/mipmap-xhdpi
# src-tauri/gen/android/app/src/main/res/mipmap-xhdpi/ic_launcher.png
# src-tauri/gen/android/app/src/main/res/mipmap-xhdpi/ic_launcher_foreground.png
# src-tauri/gen/android/app/src/main/res/mipmap-xhdpi/ic_launcher_round.png
# src-tauri/gen/android/app/src/main/res/drawable
# src-tauri/gen/android/app/src/main/res/drawable/ic_launcher_background.xml
# src-tauri/gen/android/app/src/main/res/mipmap-hdpi
# src-tauri/gen/android/app/src/main/res/mipmap-hdpi/ic_launcher.png
# src-tauri/gen/android/app/src/main/res/mipmap-hdpi/ic_launcher_foreground.png
# src-tauri/gen/android/app/src/main/res/mipmap-hdpi/ic_launcher_round.png
# src-tauri/gen/android/app/.gitignore
# src-tauri/gen/android/settings.gradle
# src-tauri/gen/android/gradlew
# src-tauri/gen/android/.gitignore


cat src-tauri/gen/android/app/build.gradle.kts

# import java.util.Properties
# 
# plugins {
#     id("com.android.application")
#     id("org.jetbrains.kotlin.android")
#     id("rust")
# }
# 
# val tauriProperties = Properties().apply {
#     val propFile = file("tauri.properties")
#     if (propFile.exists()) {
#         propFile.inputStream().use { load(it) }
#     }
# }
# 
# android {
#     compileSdk = 34
#     namespace = "domain.machine.v1"
#     defaultConfig {
#         manifestPlaceholders["usesCleartextTraffic"] = "false"
#         applicationId = "domain.machine.v1"
#         minSdk = 31
#         targetSdk = 34
#         versionCode = tauriProperties.getProperty("tauri.android.versionCode", "1").toInt()
#         versionName = tauriProperties.getProperty("tauri.android.versionName", "1.0")
#     }
#     buildTypes {
#         getByName("debug") {
#             manifestPlaceholders["usesCleartextTraffic"] = "true"
#             isDebuggable = true
#             isJniDebuggable = true
#             isMinifyEnabled = false
#             packaging {                jniLibs.keepDebugSymbols.add("*/arm64-v8a/*.so")
#                 jniLibs.keepDebugSymbols.add("*/armeabi-v7a/*.so")
#                 jniLibs.keepDebugSymbols.add("*/x86/*.so")
#                 jniLibs.keepDebugSymbols.add("*/x86_64/*.so")
#             }
#         }
#         getByName("release") {
#             isMinifyEnabled = true
#             proguardFiles(
#                 *fileTree(".") { include("**/*.pro") }
#                     .plus(getDefaultProguardFile("proguard-android-optimize.txt"))
#                     .toList().toTypedArray()
#             )
#         }
#     }
#     kotlinOptions {
#         jvmTarget = "1.8"
#     }
#     buildFeatures {
#         buildConfig = true
#     }
# }
# 
# rust {
#     rootDirRel = "../../../"
# }
# 
# dependencies {
#     implementation("androidx.webkit:webkit:1.6.1")
#     implementation("androidx.appcompat:appcompat:1.6.1")
#     implementation("com.google.android.material:material:1.8.0")
#     testImplementation("junit:junit:4.13.2")
#     androidTestImplementation("androidx.test.ext:junit:1.1.4")
#     androidTestImplementation("androidx.test.espresso:espresso-core:3.5.0")
# }
# 
# apply(from = "tauri.build.gradle.kts")


cat src-tauri/gen/android/app/src/main/AndroidManifest.xml

# <?xml version="1.0" encoding="utf-8"?>
# <manifest xmlns:android="http://schemas.android.com/apk/res/android">
#     <uses-permission android:name="android.permission.INTERNET" />
# 
#     <!-- AndroidTV support -->
#     <uses-feature android:name="android.software.leanback" android:required="false" />
# 
#     <application
#         android:icon="@mipmap/ic_launcher"
#         android:label="@string/app_name"
#         android:theme="@style/Theme.machine_v1"
#         android:usesCleartextTraffic="${usesCleartextTraffic}">
#         <activity
#             android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|smallestScreenSize|screenLayout|uiMode"
#             android:launchMode="singleTask"
#             android:label="@string/main_activity_title"
#             android:name=".MainActivity"
#             android:exported="true">
#             <intent-filter>
#                 <action android:name="android.intent.action.MAIN" />
#                 <category android:name="android.intent.category.LAUNCHER" />
#                 <!-- AndroidTV support -->
#                 <category android:name="android.intent.category.LEANBACK_LAUNCHER" />
#             </intent-filter>
#         </activity>
# 
#         <provider
#           android:name="androidx.core.content.FileProvider"
#           android:authorities="${applicationId}.fileprovider"
#           android:exported="false"
#           android:grantUriPermissions="true">
#           <meta-data
#             android:name="android.support.FILE_PROVIDER_PATHS"
#             android:resource="@xml/file_paths" />
#         </provider>
#     </application>
# </manifest>


cargo tauri android dev

#         Info Detected connected device: Pixel_6_Pro_API_34 (sdk_gphone64_x86_64) with target "x86_64-linux-android"
#      Running BeforeDevCommand (`trunk serve`)
# 2025-03-26T14:04:00.881906Z  INFO 🚀 Starting trunk 0.21.9
# 2025-03-26T14:04:00.995149Z  INFO 📦 starting build
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
# 2025-03-26T14:04:02.128029Z  INFO applying new distribution
# 2025-03-26T14:04:02.129424Z  INFO ✅ success
# 2025-03-26T14:04:02.129497Z  INFO 📡 serving static assets at -> /
# 2025-03-26T14:04:02.129689Z  INFO 📡 server listening at:
# 2025-03-26T14:04:02.129701Z  INFO     🏠 http://127.0.0.1:1420/
# 2025-03-26T14:04:02.129708Z  INFO     🏠 http://[::1]:1420/
# 2025-03-26T14:04:02.129880Z  INFO     🏠 http://localhost.:1420/
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.23s
#         Info symlinking lib "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" in jniLibs dir "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/x86_64"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libandroid.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libdl.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "liblog.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libm.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libc.so"
#         Info symlink at "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/arm64-v8a/libmachine_v1_lib.so" points to "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so"
#         Info symlink at "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/x86_64/libmachine_v1_lib.so" points to "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so"
# <=========----> 75% EXECUTING [935ms]
#         Info Forwarding port 1420 with adb
# 1420
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.20s
#         Info symlinking lib "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" in jniLibs dir "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/x86_64"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libandroid.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libdl.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "liblog.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libm.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libc.so"
# Performing Streamed Install
# Success
# Starting: Intent { cmp=domain.machine.v1/.MainActivity }
# --------- beginning of main
# 03-26 11:04:10.737  6705  6705 I main.machine.v1: Late-enabling -Xcheck:jni
# 03-26 11:04:10.789  6705  6705 I main.machine.v1: Using CollectorTypeCC GC.
# 03-26 11:04:10.791  6705  6705 W main.machine.v1: Unexpected CPU variant for x86: x86_64.
# 03-26 11:04:10.791  6705  6705 W main.machine.v1: Known variants: atom, sandybridge, silvermont, goldmont, goldmont-plus, tremont, kabylake, default
# 03-26 11:04:10.845  6705  6705 W ziparchive: Unable to open '/data/app/~~sej1v5GtrQGk1xv53XyIMg==/domain.machine.v1-FulJcvABmqM2XmSRYIpUaw==/base.dm': No such file or directory
# 03-26 11:04:10.845  6705  6705 W ziparchive: Unable to open '/data/app/~~sej1v5GtrQGk1xv53XyIMg==/domain.machine.v1-FulJcvABmqM2XmSRYIpUaw==/base.dm': No such file or directory
# 03-26 11:04:11.156  6705  6705 W main.machine.v1: Accessing hidden method Landroid/view/View;->computeFitSystemWindows(Landroid/graphics/Rect;Landroid/graphics/Rect;)Z (unsupported, reflection, allowed)
# 03-26 11:04:11.157  6705  6705 W main.machine.v1: Accessing hidden method Landroid/view/ViewGroup;->makeOptionalFitsSystemWindows()V (unsupported, reflection, allowed)
# 03-26 11:04:11.169  6705  6705 W OpenGLRenderer: Unknown dataspace 0
# 03-26 11:04:11.217  6705  6705 I WebViewFactory: Loading com.google.android.webview version 113.0.5672.136 (code 567263637)
# 03-26 11:04:11.226  6705  6705 W ziparchive: Unable to open '/data/app/~~ZMokm1r5X2JYrWNQcy1rig==/com.google.android.trichromelibrary_567263637-blK1JtZidnPOI1WHjz-erA==/TrichromeLibrary.dm': No such file or directory
# 03-26 11:04:11.226  6705  6705 W ziparchive: Unable to open '/data/app/~~ZMokm1r5X2JYrWNQcy1rig==/com.google.android.trichromelibrary_567263637-blK1JtZidnPOI1WHjz-erA==/TrichromeLibrary.dm': No such file or directory
# 03-26 11:04:11.226  6705  6705 W main.machine.v1: Entry not found
# 03-26 11:04:11.327  6705  6705 W main.machine.v1: Accessing hidden method Landroid/os/Trace;->isTagEnabled(J)Z (unsupported, reflection, allowed)
# 03-26 11:04:11.331  6705  6705 W main.machine.v1: Accessing hidden method Landroid/os/Trace;->traceBegin(JLjava/lang/String;)V (unsupported, reflection, allowed)
# 03-26 11:04:11.331  6705  6705 W main.machine.v1: Accessing hidden method Landroid/os/Trace;->traceEnd(J)V (unsupported, reflection, allowed)
# 03-26 11:04:11.331  6705  6705 W main.machine.v1: Accessing hidden method Landroid/os/Trace;->asyncTraceBegin(JLjava/lang/String;I)V (unsupported, reflection, allowed)
# 03-26 11:04:11.331  6705  6705 W main.machine.v1: Accessing hidden method Landroid/os/Trace;->asyncTraceEnd(JLjava/lang/String;I)V (unsupported, reflection, allowed)
# 03-26 11:04:11.335  6705  6705 I cr_WVCFactoryProvider: Loaded version=113.0.5672.136 minSdkVersion=29 isBundle=false multiprocess=true packageId=2
# 03-26 11:04:11.348  6705  6773 I cr_VariationsUtils: Failed reading seed file "/data/user/0/domain.machine.v1/app_webview/variations_seed_new"
# 03-26 11:04:11.348  6705  6773 I cr_VariationsUtils: Failed reading seed file "/data/user/0/domain.machine.v1/app_webview/variations_seed"
# 03-26 11:04:11.365  6705  6705 I cr_LibraryLoader: Successfully loaded native library
# 03-26 11:04:11.367  6705  6705 I cr_CachingUmaRecorder: Flushed 8 samples from 8 histograms.
# 03-26 11:04:11.493  6705  6779 W chromium: [WARNING:dns_config_service_android.cc(115)] Failed to read DnsConfig.
# 03-26 11:04:11.493  6705  6746 I RustStdoutStderr: [WARNING:dns_config_service_android.cc(115)] Failed to read DnsConfig.
# 03-26 11:04:11.697  6705  6735 W OpenGLRenderer: Failed to choose config with EGL_SWAP_BEHAVIOR_PRESERVED, retrying without...
# 03-26 11:04:11.697  6705  6735 W OpenGLRenderer: Failed to initialize 101010-2 format, error = EGL_SUCCESS
# 03-26 11:04:11.725  6705  6735 I Gralloc4: mapper 4.x is not supported
# 03-26 11:04:11.830  6705  6735 E OpenGLRenderer: Unable to match the desired swap behavior.
# 03-26 11:04:11.894  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name position
# 03-26 11:04:11.894  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name color
# 03-26 11:04:11.922  6705  6731 I OpenGLRenderer: Davey! duration=731ms; Flags=1, FrameTimelineVsyncId=47802, IntendedVsync=1331392234348, Vsync=1331842234330, InputEventId=0, HandleInputStart=1331851612967, AnimationStart=1331851623870, PerformTraversalsStart=1331851628284, DrawStart=1332041130301, FrameDeadline=1331408901014, FrameInterval=1331851218588, FrameStartTime=16666666, SyncQueued=1332090169758, SyncStart=1332092233006, IssueDrawCommandsStart=1332094232132, SwapBuffers=1332106089587, FrameCompleted=1332125796702, DequeueBufferDuration=5231, QueueBufferDuration=436182, GpuCompleted=1332125796702, SwapBuffersCompleted=1332111561381, DisplayPresentTime=0, CommandSubmissionCompleted=1332106089587,
# 03-26 11:04:11.943  6705  6798 W cr_media: BLUETOOTH_CONNECT permission is missing.
# 03-26 11:04:11.946  6705  6798 W cr_media: registerBluetoothIntentsIfNeeded: Requires BLUETOOTH permission
# 03-26 11:04:12.511  6705  6705 W Tauri/Console: File: http://tauri.localhost/ - Line 20 - Msg: The `integrity` attribute is currently ignored for preload destinations that do not support subresource integrity. See https://crbug.com/981419 for more information
#         Info Watching /home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri for changes...
#         Info Watching /home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri for changes...
# 03-26 11:04:12.990  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name inPosition
# 03-26 11:04:12.990  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name inColor
# 03-26 11:04:12.990  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name inTextureCoords
# 03-26 11:04:13.006  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name inPosition
# 03-26 11:04:13.006  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name inColor
# 03-26 11:04:13.006  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name inCircleEdge
# 03-26 11:04:13.011  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name position
# 03-26 11:04:13.011  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name localCoord
# 03-26 11:04:13.019  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name position
# 03-26 11:04:13.019  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name localCoord
# 03-26 11:04:13.025  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name position
# 03-26 11:04:13.025  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name color
# 03-26 11:04:13.034  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name inPosition
# 03-26 11:04:13.034  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name inColor
# 03-26 11:04:13.035  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name inCircleEdge
# 03-26 11:04:13.046  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name position
# 03-26 11:04:13.046  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name localCoord
# 03-26 11:04:13.055  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name inputPoint
# 03-26 11:04:13.062  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name resolveLevel_and_idx
# 03-26 11:04:13.062  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name p01
# 03-26 11:04:13.062  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name p23
# 03-26 11:04:13.070  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name position
# 03-26 11:04:13.075  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name color
# 03-26 11:04:13.078  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name fillBounds
# 03-26 11:04:13.078  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name color
# 03-26 11:04:13.079  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name locations
# 03-26 11:04:13.084  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name inPosition
# 03-26 11:04:13.107  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name inPosition
# 03-26 11:04:13.107  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name inColor
# 03-26 11:04:13.108  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name inQuadEdge
# 03-26 11:04:13.114  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 0 name radii_selector
# 03-26 11:04:13.115  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 1 name corner_and_radius_outsets
# 03-26 11:04:13.115  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 2 name aa_bloat_and_coverage
# 03-26 11:04:13.115  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 3 name radii_x
# 03-26 11:04:13.115  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 4 name radii_y
# 03-26 11:04:13.115  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 5 name skew
# 03-26 11:04:13.116  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 6 name translate_and_localrotate
# 03-26 11:04:13.116  6705  6746 I RustStdoutStderr: s_glBindAttribLocation: bind attrib 7 name color
# [...]


# https://developer.android.com/studio/run/device


adb devices -l

# List of devices attached
# RQ8MA0WKXCL            device usb:1-2 product:d2sxx model:SM_N975F device:d2s transport_id:


cargo tauri android dev

#         Info Detected connected device: A19 (SM-N975F) with target "aarch64-linux-android"
#         Info Using 192.168.72.152 to access the development server.
#         Info Replacing devUrl host with 192.168.72.152. If your frontend is not listening on that address, try configuring your development server to use the `TAURI_DEV_HOST` environment variable or 0.0.0.0 as host.
#      Running BeforeDevCommand (`trunk serve`)
# 2025-03-26T14:00:09.355782Z  INFO 🚀 Starting trunk 0.21.9
# 2025-03-26T14:00:09.438380Z  INFO 📦 starting build
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.09s
# 2025-03-26T14:00:10.203968Z  INFO applying new distribution
# 2025-03-26T14:00:10.205179Z  INFO ✅ success
# 2025-03-26T14:00:10.205239Z  INFO 📡 serving static assets at -> /
# 2025-03-26T14:00:10.205446Z  INFO 📡 server listening at:
# 2025-03-26T14:00:10.205459Z  INFO     💻 http://192.168.72.152:1420/
# 2025-03-26T14:00:10.242488Z  INFO     💻 http://L24.:1420/
# 2025-03-26T14:00:10.242504Z  INFO     💻 http://L24.local.:1420/
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
#         Info symlinking lib "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" in jniLibs dir "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/arm64-v8a"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libandroid.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libdl.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "liblog.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libm.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libc.so"
#         Info symlink at "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/arm64-v8a/libmachine_v1_lib.so" points to "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so"
#         Info symlink at "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/x86_64/libmachine_v1_lib.so" points to "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/x86_64-linux-android/debug/libmachine_v1_lib.so"
# <======-------> 53% EXECUTING [503ms]
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.14s
#         Info symlinking lib "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" in jniLibs dir "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/arm64-v8a"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libandroid.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libdl.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "liblog.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libm.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libc.so"
# Performing Streamed Install
# Success
# Starting: Intent { cmp=domain.machine.v1/.MainActivity }
#         Info Watching /home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri for changes...
#         Info Watching /home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri for changes...
# --------- beginning of main
# 03-26 11:00:21.951 16743 16743 I main.machine.v: Late-enabling -Xcheck:jni
# 03-26 11:00:21.992 16743 16743 E USNET   : USNET: appName: domain.machine.v1
# 03-26 11:00:22.233 16743 16833 I libEGL  : EGL_ANDROID_blob_cache_path advertised, but unable to get eglSetBlobCachePathANDROID
# 03-26 11:00:22.374 16743 16743 I DecorView: [INFO] isPopOver=false, config=true
# 03-26 11:00:22.374 16743 16743 I DecorView: updateCaptionType >> DecorView@add5b47[], isFloating=false, isApplication=true, hasWindowControllerCallback=true, hasWindowDecorCaption=false
# 03-26 11:00:22.381 16743 16743 I DecorView: getCurrentDensityDpi: from real metrics. densityDpi=560 msg=resources_loaded
# 03-26 11:00:22.404 16743 16743 W main.machine.v: Accessing hidden method Landroid/view/View;->computeFitSystemWindows(Landroid/graphics/Rect;Landroid/graphics/Rect;)Z (unsupported, reflection, allowed)
# 03-26 11:00:22.404 16743 16743 W main.machine.v: Accessing hidden method Landroid/view/ViewGroup;->makeOptionalFitsSystemWindows()V (unsupported, reflection, allowed)
# 03-26 11:00:22.407 16743 16743 I MSHandlerLifeCycle: isMultiSplitHandlerRequested: ignored. pkg=domain.machine.v1 parent=null callers=com.android.internal.policy.DecorView.setVisibility:4294 android.app.ActivityThread.handleResumeActivity:5301 android.app.servertransaction.ResumeActivityItem.execute:54 android.app.servertransaction.ActivityTransactionItem.execute:45 android.app.servertransaction.TransactionExecutor.executeLifecycleState:176
# 03-26 11:00:22.407 16743 16743 I MSHandlerLifeCycle: removeMultiSplitHandler: no exist. decor=DecorView@add5b47[]
# 03-26 11:00:22.419 16743 16743 I ViewRootImpl@ba35be[MainActivity]: setView = com.android.internal.policy.DecorView@add5b47 TM=true
# 03-26 11:00:22.420 16743 16743 I MSHandlerLifeCycle: isMultiSplitHandlerRequested: windowingMode=1 isFullscreen=true isPopOver=false isHidden=false skipActivityType=false isHandlerType=false this: DecorView@add5b47[MainActivity]
# 03-26 11:00:22.420 16743 16743 I MSHandlerLifeCycle: removeMultiSplitHandler: no exist. decor=DecorView@add5b47[MainActivity]
# 03-26 11:00:22.439 16743 16743 I ViewRootImpl@ba35be[MainActivity]: Relayout returned: old=(0,0,1440,3040) new=(0,0,1440,3040) req=(1440,3040)0 dur=5 res=0x7 s={true 512428755680} ch=true fn=-1
# 03-26 11:00:22.440 16743 16743 I ViewRootImpl@ba35be[MainActivity]: [DP] dp(1) 1 android.view.ViewRootImpl.reportNextDraw:11420 android.view.ViewRootImpl.performTraversals:4193 android.view.ViewRootImpl.doTraversal:2919
# 03-26 11:00:22.443 16743 16869 I Gralloc4: mapper 4.x is not supported
# 03-26 11:00:22.443 16743 16869 W Gralloc3: mapper 3.x is not supported
# 03-26 11:00:22.444 16743 16869 I gralloc : Arm Module v1.0
# 03-26 11:00:22.446 16743 16869 W Gralloc4: allocator 4.x is not supported
# 03-26 11:00:22.446 16743 16869 W Gralloc3: allocator 3.x is not supported
# 03-26 11:00:22.471 16743 16743 I WebViewFactory: Loading com.google.android.webview version 134.0.6998.135 (code 699813533)
# 03-26 11:00:22.473 16743 16743 W main.machine.v: Entry not found
# 03-26 11:00:22.493 16743 16743 I cr_WVCFactoryProvider: version=134.0.6998.135 (699813533) minSdkVersion=29 isBundle=false multiprocess=true packageId=2
# 03-26 11:00:22.502 16743 16879 I chromium: [0326/110022.502103:INFO:variations_seed_loader.cc(67)] Failed to open file for reading.: No such file or directory (2)
# 03-26 11:00:22.502 16743 16856 I RustStdoutStderr: [0326/110022.502103:INFO:variations_seed_loader.cc(67)] Failed to open file for reading.: No such file or directory (2)
# 03-26 11:00:22.502 16743 16879 I chromium: [0326/110022.502442:INFO:variations_seed_loader.cc(67)] Failed to open file for reading.: No such file or directory (2)
# 03-26 11:00:22.502 16743 16856 I RustStdoutStderr: [0326/110022.502442:INFO:variations_seed_loader.cc(67)] Failed to open file for reading.: No such file or directory (2)
# 03-26 11:00:22.513 16743 16743 I cr_LibraryLoader: Successfully loaded native library
# 03-26 11:00:22.514 16743 16743 I cr_CachingUmaRecorder: Flushed 6 samples from 6 histograms, 0 samples were dropped.
# 03-26 11:00:22.516 16743 16743 I cr_CombinedPProvider: #registerProvider() provider:WV.I8@af77359 isPolicyCacheEnabled:false policyProvidersSize:0
# 03-26 11:00:22.517 16743 16743 I cr_PolicyProvider: #setManagerAndSource() 0
# 03-26 11:00:22.536 16743 16743 I cr_CombinedPProvider: #linkNativeInternal() 1
# 03-26 11:00:22.538 16743 16743 I cr_AppResProvider: #getApplicationRestrictionsFromUserManager() Bundle[EMPTY_PARCEL]
# 03-26 11:00:22.538 16743 16743 I cr_PolicyProvider: #notifySettingsAvailable() 0
# 03-26 11:00:22.538 16743 16743 I cr_CombinedPProvider: #onSettingsAvailable() 0
# 03-26 11:00:22.538 16743 16743 I cr_CombinedPProvider: #flushPolicies()
# 03-26 11:00:22.566 16743 16899 W chromium: [WARNING:dns_config_service_android.cc(81)] Failed to read DnsConfig.
# 03-26 11:00:22.566 16743 16856 I RustStdoutStderr: [WARNING:dns_config_service_android.cc(81)] Failed to read DnsConfig.
# 03-26 11:00:22.661 16743 16743 I ViewRootImpl@ba35be[MainActivity]: [DP] pdf(0) 1 android.view.ViewRootImpl.lambda$addFrameCompleteCallbackIfNeeded$3$ViewRootImpl:4995 android.view.ViewRootImpl$$ExternalSyntheticLambda16.run:6 android.os.Handler.handleCallback:938
# 03-26 11:00:22.661 16743 16743 I ViewRootImpl@ba35be[MainActivity]: [DP] rdf()
# 03-26 11:00:22.664 16743 16961 I CameraManagerGlobal: Connecting to camera service
# 03-26 11:00:22.671 16743 16797 I CameraManagerGlobal: Camera 0 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.672 16743 16797 I CameraManagerGlobal: Camera 1 facing CAMERA_FACING_FRONT state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.675 16743 16797 I CameraManagerGlobal: Camera 2 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.677 16743 16797 I CameraManagerGlobal: Camera 20 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.679 16743 16767 I CameraManagerGlobal: Camera 21 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.680 16743 16767 I CameraManagerGlobal: Camera 23 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.682 16743 16767 I CameraManagerGlobal: Camera 3 facing CAMERA_FACING_FRONT state now CAMERA_STATE_CLOSED for client org.mozilla.firefox API Level 2
# 03-26 11:00:22.682 16743 16767 I CameraManagerGlobal: Camera 4 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.682 16743 16767 I CameraManagerGlobal: Camera 40 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.683 16743 16767 I CameraManagerGlobal: Camera 50 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.684 16743 16767 I CameraManagerGlobal: Camera 52 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.684 16743 16767 I CameraManagerGlobal: Camera 80 facing CAMERA_FACING_BACK state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.684 16743 16767 I CameraManagerGlobal: Camera 91 facing CAMERA_FACING_FRONT state now CAMERA_STATE_CLOSED for client android.system API Level 2
# 03-26 11:00:22.697 16743 16961 I CameraManager: registerAvailabilityCallback: Is device callback = false
# 03-26 11:00:22.697 16743 16961 I CameraManagerGlobal: postSingleUpdate device: camera id 0 status STATUS_PRESENT
# 03-26 11:00:22.698 16743 16961 I CameraManagerGlobal: postSingleUpdate device: camera id 1 status STATUS_PRESENT
# 03-26 11:00:22.698 16743 16961 I CameraManagerGlobal: postSingleUpdate device: camera id 2 status STATUS_PRESENT
# 03-26 11:00:22.698 16743 16961 I CameraManagerGlobal: postSingleUpdate device: camera id 3 status STATUS_PRESENT
# 03-26 11:00:22.698 16743 16961 I CameraManagerGlobal: postSingleUpdate device: camera id 4 status STATUS_PRESENT
# 03-26 11:00:22.743 16743 16977 W chromium: [WARNING:viz_main_impl.cc(85)] VizNullHypothesis is disabled (not a warning)
# 03-26 11:00:22.744 16743 16856 I RustStdoutStderr: [WARNING:viz_main_impl.cc(85)] VizNullHypothesis is disabled (not a warning)
# 03-26 11:00:22.752 16743 16743 I ViewRootImpl@ba35be[MainActivity]: MSG_WINDOW_FOCUS_CHANGED 1 1
# 03-26 11:00:22.757 16743 16921 W cr_media: BLUETOOTH_CONNECT permission is missing.
# 03-26 11:00:22.759 16743 16921 W cr_media: getBluetoothAdapter() requires BLUETOOTH permission
# 03-26 11:00:22.759 16743 16921 W cr_media: registerBluetoothIntentsIfNeeded: Requires BLUETOOTH permission
# 03-26 11:00:22.759 16743 16743 I InputMethodManager: startInputInner - mService.startInputOrWindowGainedFocus
# 03-26 11:00:22.883 16743 16931 W AudioCapabilities: Unsupported mime audio/x-ape
# 03-26 11:00:22.885 16743 16931 W AudioCapabilities: Unsupported mime audio/x-ima
# 03-26 11:00:22.887 16743 16931 W AudioCapabilities: Unsupported mime audio/mpeg-L1
# 03-26 11:00:22.888 16743 16931 W AudioCapabilities: Unsupported mime audio/mpeg-L2
# 03-26 11:00:22.888 16743 16931 W VideoCapabilities: Unsupported mime video/mp43
# 03-26 11:00:22.888 16743 16931 W VideoCapabilities: Unsupported mime video/wvc1
# 03-26 11:00:22.889 16743 16931 W VideoCapabilities: Unsupported mime video/x-ms-wmv
# 03-26 11:00:22.889 16743 16931 W AudioCapabilities: Unsupported mime audio/x-ms-wma
# 03-26 11:00:22.889 16743 16931 W VideoCapabilities: Unsupported mime video/x-ms-wmv7
# 03-26 11:00:22.890 16743 16931 W VideoCapabilities: Unsupported mime video/x-ms-wmv8
# 03-26 11:00:23.084 16743 16743 W Tauri/Console: File: http://tauri.localhost/ - Line 20 - Msg: The `integrity` attribute is currently ignored for preload destinations that do not support subresource integrity. See https://crbug.com/981419 for more information
# 03-26 11:00:28.676 16743 16750 I main.machine.v: Thread[4,tid=16750,WaitingInMainSignalCatcherLoop,Thread*=0x773f209f50,peer=0x13a40228,"Signal Catcher"]: reacting to signal 10
# 03-26 11:00:28.676 16743 16750 I main.machine.v:
# 03-26 11:00:28.676 16743 16750 I main.machine.v: SIGUSR1 forcing GC (no HPROF) and profile save
# 03-26 11:00:28.698 16743 16750 I main.machine.v: Explicit concurrent copying GC freed 8398(481KB) AllocSpace objects, 1(956KB) LOS objects, 58% free, 4438KB/10MB, paused 42us,30us total 22.338ms
# [...]



cargo tauri android build --help

# Build your app in release mode for Android and generate APKs and AABs. It makes use of the `build.frontendDist` property from your `tauri.conf.json` file. It also runs your `build.beforeBui
# 
# Usage: cargo tauri android build [OPTIONS]
# 
# Options:
#   -d, --debug
#           Builds with the debug flag
# 
#   -v, --verbose...
#           Enables verbose logging
# 
#   -t, --target [<TARGETS>...]
#           Which targets to build (all by default)
# 
#           [possible values: aarch64, armv7, i686, x86_64]
# 
#   -f, --features [<FEATURES>...]
#           List of cargo features to activate
# 
#   -c, --config <CONFIG>
#           JSON strings or path to JSON files to merge with the default configuration file
# 
#           Configurations are merged in the order they are provided, which means a particular value overwrites previous values when a config key-value pair conflicts.
# 
#           Note that a platform-specific file is looked up and merged with the default file by default (tauri.macos.conf.json, tauri.linux.conf.json, tauri.windows.conf.json, tauri.android.c
# 
#       --split-per-abi
#           Whether to split the APKs and AABs per ABIs
# 
#       --apk
#           Build APKs
# 
#       --aab
#           Build AABs
# 
#   -o, --open
#           Open Android Studio
# 
#       --ci
#           Skip prompting for values
# 
#           [env: CI=]
# 
#   -h, --help
#           Print help (see a summary with '-h')
# 
#   -V, --version
#           Print version


cargo tauri android build --debug --apk --target aarch64

#      Running beforeBuildCommand `trunk build`
# 2025-03-26T16:14:33.690616Z  INFO 🚀 Starting trunk 0.21.9
# 2025-03-26T16:14:33.690991Z  INFO 📦 starting build
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
# 2025-03-26T16:14:34.639308Z  INFO applying new distribution
# 2025-03-26T16:14:34.640946Z  INFO ✅ success
#    Compiling tauri v2.4.0
#    Compiling tauri-macros v2.1.0
#    Compiling tauri-plugin-opener v2.2.6
#    Compiling machine-v1 v0.0.1 (/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri)
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 7.84s
#         Info symlinking lib "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" in jniLibs dir "/home/cavani/Workspace/prog
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libandroid.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libdl.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "liblog.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libm.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libc.so"
#         Info symlink at "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/arm64-v8a/libmachine_v1_lib.so" points to "/home/cavani/Works
#         Info symlink at "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/x86_64/libmachine_v1_lib.so" points to "/home/cavani/Workspac
# <========-----> 63% EXECUTING [3s]
#    Compiling machine-v1 v0.0.1 (/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri)eClasspath'
#     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.94s
#         Info symlinking lib "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" in jniLibs dir "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/src/main/jniLibs/arm64-v8a"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libandroid.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libdl.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "liblog.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libm.so"
#         Info "/home/cavani/Workspace/programmable-matter-tauri/machine-v1/target/aarch64-linux-android/debug/libmachine_v1_lib.so" requires shared lib "libc.so"
#     Finished 1 APK at:
#         /home/cavani/Workspace/programmable-matter-tauri/machine-v1/src-tauri/gen/android/app/build/outputs/apk/universal/debug/app-universal-debug.apk


adb devices -l

# List of devices attached
# RQ8MA0WKXCL            device usb:1-2 product:d2sxx model:SM_N975F device:d2s transport_id:6


adb -d install src-tauri/gen/android/app/build/outputs/apk/universal/debug/app-universal-debug.apk

# Performing Streamed Install
# Success
```

## Leptos Web UI

```sh


```
