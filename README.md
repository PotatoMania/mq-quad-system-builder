# MQ Quad System Builder

Nothing special, just scripts to build a system image for MQ Quad.

Working progress.

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

### Build Linux

WIP
