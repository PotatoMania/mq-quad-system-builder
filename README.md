# MQ Quad System Builder

Nothing special, just scripts to build a system image for MQ Quad.

Working progress. Now ArchLinux boots on MQ Quad

## How to use

### Build U-Boot

Please patch against u-boot commit `e508b930021168c788f14977fc101ccc1151b3c8`.

Automatic patching WIP

```bash
cd u-boot
make prepare-code-atf
make prepare-code-u-boot
cd u-boot && git am ../patches/* && cd ..
make build-u-boot -j$(nproc)
```

Now you have `u-boot/u-boot/u-boot-sunxi-with-spl.bin`. Write it to your target device:

```bash
dd if=u-boot-sunxi-with-spl.bin bs=8K seek=1 of=/dev/targetdisk
```

_If you're using GPT partition tables, please shrink its size to below or equal to 56,_
_so that u-boot won't be overwritten or vice-versa._
_This can be done with gdisk's expert mode._
_Read [sunxi community wiki](https://linux-sunxi.org/Bootable_SD_card#GPT_.28experimental.29) for more information._

### Build Linux

WIP

You just need to add the devicetree and create a config. Configuration is the hard part.

Using EFI GRUB to boot the kernel is a validated method. You can also directly boot with u-boot, of course.

```
pacman -S base-devel
```