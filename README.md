# Crownstone SDK

The Crownstone SDK exists of three parts (in an increasing order of integration):

:cloud: A [REST API](#rest_api) in the cloud

:iphone: Smartphone [libraries](#smartphone_libs)

:crown: The [firmware](#bluenet) on the Crownstones, called Bluenet

# <a name="rest_api"></a>REST API

The cloud is required to setup the Crownstones: keys and IDs will be generated, and locations can be set.
After that, it can be used to add users, so they can also make use of your Crownstones.
The cloud is also used to synchronize data between users, and serves as data storage.
You can read how to use it in the [REST API documentation](REST_API.md).

![Image of Strongloop API Explorer](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/strongloop-api-explorer.png)

# <a name="smartphone_libs"></a>Smartphone libraries
To make things easy, we provide native libraries for smartphones. The following libraries are available and can be found on github:

- [Android](https://github.com/crownstone/bluenet-lib-android)
- [iOS](https://github.com/crownstone/bluenet-lib-ios)

The libraries abstract the communication with the Crownstones. They simplify scanning/search for crownstones, wrap the messages into easy-to-use objects, and provide simple functions to access the functionalities provided on the Crownstones. 

The following features will be available (some are still in development):

## <a name="bluenet_lib_commands"></a>Commands

- Switch/dim
- Set time
- Factory reset
- Update over the air
- Enable/disable iBeacon
- Enable/disable encryption
- Enable/disable mesh
- Enable/disable continous scanning
- Enable/disable continous high frequency power sampling

## Notified data
This data streams in regularly via a callback.

- Switch state (0-100)
- Power usage (mW)
- Total energy usage (Wh)
- Chip temperature (°C)

## <a name="bluenet_lib_configs"></a>Get/set configurations:

Configurations that can be set and read. 

Note: The enable/disable states can only be set using the corresponding [command](#bluenet_lib_commands) but they can be read through the config.

- Encryption keys
- ID
- iBeacon UUID, major, minor, RSSI at 1m
- TX power
- Advertisement interval
- Schedule (switch on/off at certain times)
- Toggle switch after Crownstone reboot.
- Continous scanning interval, duration and filter

## Mesh commands

Commands that can be issued to other Crownstones via the mesh. In case a command asks for a return value, the value will be notified via a callback.

- Switch
- Get state (switch state, power usage, energy usage, temperature)
- Get [config](#bluenet_lib_configs)
- Set [config](#bluenet_lib_configs)
- Enable/disable scanning or high frequency power sampling


## Indoor localization

![Image of Indoor Localization](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/indoor-localization.png)

This library abstracts and implements the localization, it uses the [bluetooth library](#bluenet_lib) and [REST API](#rest_api). The localization is based on rooms, though it is more a certain area. This means you can define multiple rooms in a single real world room.

Features (in development):

- Create room
- Remove room
- Start/stop learning room
- Set room fingerprint
- Get room fingerprint
- Adapt a fingerprint from the cloud to your phone model
- Get current location (room)
- Get nearby rooms
- Get distance to room
- Get predicted next room
- Get location (room) of other users

## Smartphone app

![Image of Example app](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/crownstone-app-small.png)
![Second image of Example app](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/crownstone-app-small1.png)
![Third image of Example app](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/crownstone-app-small2.png)
![Fourth image of Example app](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/crownstone-app-small3.png)
![Fifth image of Example app](https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/crownstone-app-small4.png)

The [Crownstone app](https://github.com/crownstone/CrownstoneApp) (Android and iOS) makes use of these libraries and connects to the REST API as well.
The Crownstone app can be used as starting point to develop your own stand-alone app.
It is written in React Native.

# <a name="bluenet"></a>Bluenet Firmware

The [Bluenet](https://github.com/crownstone/bluenet/) firmware can be downloaded from github. For the documentation, see the following links

- [Bluetooth Protocol](https://github.com/crownstone/bluenet/blob/master/docs/PROTOCOL.md)
Protocol description of the services, characteristics, advertisements, and mesh.
- [Installation Manual](https://github.com/crownstone/bluenet/blob/master/docs/INSTALL.md) 
A step by step description to install the build system required to build and run the bluenet firmware.
- [License](https://github.com/crownstone/bluenet/blob/master/LICENSE.txt)
License Agreement

# <a name="bootloader"></a>Bootloader

We are using a modified version of the bootloader from the Nordic SDK which can be found [here](https://github.com/crownstone/bluenet-bootloader). 

The bootloader handles starting the firmware, as well as device firmware upload (DFU) over the air. It provides DFU for firmware, softdevice and bootloader. In addition, the bootloader also provides some checks to handle unwanted resets, which means it will automatically go into DFU mode when too many subsequent resets are detected.

Note: The bootloader is optional if the Crownstones are programmed over USB. It is only needed to be able to DFU over the air.

# <a name="bluenet_installation"></a>Installation (In progress)

The installation of the bluenet software and related utilities can be done by cmake as well. First create a workspace,
say `crownstone`, then clone the different repositories from within this directory.

	mkdir -p crownstone && cd crownstone
	git clone git@github:crownstone/crownstone-sdk sdk
	cd sdk
	mkdir -p build && cd build
	cmake ..
	make

This will create a `crownstone/source` directory and download the bluenet source code. After this it will be compiled.

For now, if there no proper `crownstone/config/${target}/CMakeBuild.config` file, the build will fail. Copy it
manually from the source repository and adjust its values according to your own system.

# <a name="bluenet_installation"></a>Installation (Legacy)

To simplify installation of the Bluenet build system on linux, an `install.sh` script is provided in this repository which downloads and installs everything required to build the Bluenet firmware. 

Note: By using the script you implicitly accept the terms of the license agreement of the JLink/Segger. For the license agreement, see [here](https://www.segger.com/downloads/jlink/jlink_5.12.8_x86_64.deb).

To use it, simply copy the script to the root location where you want to download the the files, e.g.

    cd ~/Project/Crownstone/
    
Then run the script with

    ./install.sh
    
It will download all necessary dependencies, clone into the Bluenet repository, and set up the config/build system to start programing straight away.

If you prefer to install it manually, you can find a step by step installation manual [here](https://github.com/crownstone/bluenet/blob/master/INSTALL.md).


# <a name="hardware"></a>Crownstone Hardware

You can order the Crownstone hardware at the [shop](https://shop.crownstone.rocks). To develop your own firmware you can either (1) perform over-the-air updates via Bluetooth (recommended), (2) have a wired setup using a Nordic development board (more hardcore), and (3) have a wired setup using a Crownstone itself (very hardcore).

Updates over-the-air can be performed by for example the [nRF Toolbox App](https://www.nordicsemi.com/eng/Products/Nordic-mobile-Apps/nRF-Toolbox-App) from Nordic.

A wired development setup using a Nordic development board (a) will give you information over UART and (b) allows for faster development cycles with respect to uploading the firmware. For this we recommend you to buy the following:

* a JTAG programmer/debugger (we use the [JLink](https://www.segger.com/products/debug-probes/j-link/models/j-link-base/) priced at $300).
* the Nordic development kit ([nRF 52 DK](https://www.nordicsemi.com/eng/Products/Bluetooth-low-energy/nRF52-DK) priced at $40).

A wired setup using a Crownstone itself is very hardcore, because you need to make sure you are isolated from the grid. The "Crownstone in the loop" setup has as advantages above a Nordic dev board that (a) it can actually switch and dim devices, (b) measure actual current/voltage curves, and (c) stream this high-bandwidth data to the attached computer. You will need the following:

* a JTAG programmer/debugger (we use the [JLink](https://www.segger.com/products/debug-probes/j-link/models/j-link-base/) priced at $300).
* a connector that fits the JTAG programmer on one side and 4 wires on the other side (ask us for instructions).

<img align="right" src="https://raw.githubusercontent.com/crownstone/crownstone-sdk/master/images/pin-layout.png" width="20%" alt="Pin layout on the Crownstone printed circuit board (PCB)">

Note! If you use the JLink, enable uart/serial manually:

    JLinkExe
    > vcom enable
    
Unplug the device and plug it back in. (Crownstone does not need to be attached). Only now you will see it pop-up as `/dev/ttyACM0` (given you've used their `/etc/udev/rules.d/99-jlink.rules` file as well).

For pros. If you want to power something from the JLink as well. You can set the JLink such that it provides power rather than checks for power.

    JLinkExe
    > power on perm

The Crownstones have pins that allow them to be programmed by wire. You will need to solder four wires to the pins labelled P1-P6. Make absolutely sure that you are doubly isolated if the Crownstone is powered via the grid (either 220V/110V). For example, a possible setup is the following: have a Raspberry PI connected via wires to the Crownstone and power the Raspberry PI via a doubly isolated adapter (recognizable by a square inside a square symbol). Reach the Raspberry PI via Wifi (so program it remotely). In this way, you don't risk your development station.

The pin layout for the ACR01B1D board (the type is written on the PCB):

* P1 is (virtual) ground
* P2 is 3V3
* P3 is SWD-clk
* P4 is SWD-IO
* P5 is GPIO, currently used as UART TX by the firmware
* P6 is GPIO, currently used as UART RX by the firmware

Be extremely cautious of course!

