# fusee-lede
Instructions/files for building a custom LEDE image to turn cheap routers into a Nintendo Switch "modchip"/"dongle"

These instructions target the "A5-V11" devices, although you should be able to make this work on any router with USB host support.

These instructions do not cover the actual installation to the device, see https://openwrt.org/toh/unbranded/a5-v11

# Usage

Once installed, just plug in your switch in RCM mode, and the payload will get launched automagically!

To set a custom payload, replace `/usr/share/fusee-nano/payload.bin`. (`fusee.bin` is bundled as a default payload, from https://github.com/ktemkin/Atmosphere/tree/poc_nvidia/fusee)

# Compiling From Source

1. Clone this repo, and the main LEDE repo

```sh
git clone https://github.com/DavidBuchanan314/fusee-lede/
git clone -b lede-17.01 https://git.lede-project.org/source.git lede
```

2. Copy over the `fusee-nano` package, and the EHCI patch

```sh
cp -r fusee-lede/fusee-nano lede/package/utils/
cp fusee-lede/899-ehci_enable_large_ctl_xfers.patch lede/target/linux/generic/patches-4.4/
```

3. Update LEDE feeds and configure

```
cd lede
./scripts/feeds update -a
./scripts/feeds install -a

make menuconfig
```

If you run into issues here, refer to https://wiki.openwrt.org/doc/howto/build

When in the configuration menu, you will need to set the following options:

```
Target System            => MediaTek Ralink MIPS
Subtarget                => RT3x5x/RT5350 based boards
Target Profile           => A5-V11
Utilities > fusee-nano   => <M>
```

4. Compile!

```
make -j12
```
If all goes well, you should find files ready to flash here:

```
bin/targets/ramips/rt305x/lede-ramips-rt305x-a5-v11-squashfs-factory.bin
bin/targets/ramips/rt305x/lede-ramips-rt305x-a5-v11-squashfs-sysupgrade.bin
```
