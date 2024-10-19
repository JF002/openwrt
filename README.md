**The original content of README.md is available below)**

# OpenWRT on Pine64 PineDio LoRa GW
This fork add support for the [Pine64 PineDio LoRa Gateway](https://pine64.org/documentation/Pinedio/Gateway/) in OpenWRT. This fork is based on [OpenWRT 23.05.05](https://github.com/openwrt/openwrt/tree/v23.05.5).

The following changes are needed for the LoRa Packet Forwarder to run and connect to [The Things Network](https://www.thethingsnetwork.org/):
 - [PATCH](https://github.com/JF002/openwrt/blob/pinedio-lora-gw/target/linux/sunxi/patches-5.15/999-pine64-lora-gw.patch) Edit the DTS file to enable SPI0 and UART2. Those peripherals are needed to communicate with the LoRa module and its GPS.
 - Modify the package `sx1302_hal` to build the packet_forwarder application and to fix 2 issues
   - [Makefile](https://github.com/JF002/openwrt-pinedio-lora-gw-custom-feed/blob/master/sx1302_hal_pine64/Makefile#L93) Disable the temperature sensor which does not seems to be available on our board (to be confirmed)
   - [PATCH](https://github.com/JF002/openwrt-pinedio-lora-gw-custom-feed/blob/master/sx1302_hal_pine64/patches/999-fix-and-deploy-packet-forwarder.patch) Move a big buffer from the stach to the heap to avoid a SEFGAULT (the size of the stack on OpenWRT is probably smaller than on regular Linux OS).

## How to build
Follow the [original instructions from OpenWRT](https://github.com/openwrt/openwrt?tab=readme-ov-file#quickstart). The only additional step consists in copying `.config_pine64_pinedio_lora_gw` into .config to ensure that all options and settings are correctly applied to work with the LoRa gateway.

```sh
git clone git@github.com:JF002/openwrt.git -b pinedio-lora-gw
cd openwrt
./scripts/feeds update -a
./scripts/feeds install -a
cp .config_pine64_pinedio_lora_gw .config
make 
```

## How to run the lora packet forwarder?
<TODO>

## How to run other test programs ?
<TODO>

## TODO
 - [ ] Automatically start the forwarder at boot time
 - [ ] Enable WiFi
 - [ ] Provide generic configuration file for the lora packet forwarder application and document how to edit them
 - [ ] Enable logs for the lora packet forwarder

---

![OpenWrt logo](include/logo.png)

OpenWrt Project is a Linux operating system targeting embedded devices. Instead
of trying to create a single, static firmware, OpenWrt provides a fully
writable filesystem with package management. This frees you from the
application selection and configuration provided by the vendor and allows you
to customize the device through the use of packages to suit any application.
For developers, OpenWrt is the framework to build an application without having
to build a complete firmware around it; for users this means the ability for
full customization, to use the device in ways never envisioned.

Sunshine!

## Download

Built firmware images are available for many architectures and come with a
package selection to be used as WiFi home router. To quickly find a factory
image usable to migrate from a vendor stock firmware to OpenWrt, try the
*Firmware Selector*.

* [OpenWrt Firmware Selector](https://firmware-selector.openwrt.org/)

If your device is supported, please follow the **Info** link to see install
instructions or consult the support resources listed below.

## 

An advanced user may require additional or specific package. (Toolchain, SDK, ...) For everything else than simple firmware download, try the wiki download page:

* [OpenWrt Wiki Download](https://openwrt.org/downloads)

## Development

To build your own firmware you need a GNU/Linux, BSD or MacOSX system (case
sensitive filesystem required). Cygwin is unsupported because of the lack of a
case sensitive file system.

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.

```
binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev
make4.1+ perl python3.6+ rsync subversion unzip which
```

### Quickstart

1. Run `./scripts/feeds update -a` to obtain all the latest package definitions
   defined in feeds.conf / feeds.conf.default

2. Run `./scripts/feeds install -a` to install symlinks for all obtained
   packages into package/feeds/

3. Run `make menuconfig` to select your preferred configuration for the
   toolchain, target system & firmware packages.

4. Run `make` to build your firmware. This will download all sources, build the
   cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen
   applications for your target system.

### Related Repositories

The main repository uses multiple sub-repositories to manage packages of
different categories. All packages are installed via the OpenWrt package
manager called `opkg`. If you're looking to develop the web interface or port
packages to OpenWrt, please find the fitting repository below.

* [LuCI Web Interface](https://github.com/openwrt/luci): Modern and modular
  interface to control the device via a web browser.

* [OpenWrt Packages](https://github.com/openwrt/packages): Community repository
  of ported packages.

* [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically
  focused on (mesh) routing.

* [OpenWrt Video](https://github.com/openwrt/video): Packages specifically
  focused on display servers and clients (Xorg and Wayland).

## Support Information

For a list of supported devices see the [OpenWrt Hardware Database](https://openwrt.org/supported_devices)

### Documentation

* [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
* [User Guide](https://openwrt.org/docs/guide-user/start)
* [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
* [Technical Reference](https://openwrt.org/docs/techref/start)

### Support Community

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advise.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## License

OpenWrt is licensed under GPL-2.0
