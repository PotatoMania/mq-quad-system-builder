## u-boot uild script

UBOOT_CODE_DIR := u-boot
ATF_CODE_DIR := arm-trusted-firmware

## Source code preparations

prepare-code-atf: $(ATF_CODE_DIR)
prepare-code-u-boot: $(UBOOT_CODE_DIR)

$(ATF_CODE_DIR):
	git clone --depth 1 https://github.com/ARM-software/arm-trusted-firmware.git $(ATF_CODE_DIR)

$(UBOOT_CODE_DIR):
	git clone --depth 1 https://github.com/u-boot/u-boot $(UBOOT_CODE_DIR)

## Source code patching


## Building


## All in one
