## u-boot build script

# general configs #
CROSS_COMPILE := aarch64-linux-gnu-
DEBUG := 1

# ATF configs #
ATF_CODE_DIR := arm-trusted-firmware
ATF_PLATFORM := sun50i_h616
ATF_EXTRA_PARAM :=

ifeq ($(DEBUG), 1)
	ATF_BINARY = $(ATF_CODE_DIR)/build/sun50i_h616/debug/bl31.bin
	ATF_EXTRA_PARAM += DEBUG=1
else
	ATF_BINARY = $(ATF_CODE_DIR)/build/sun50i_h616/release/bl31.bin
endif

# board uses AXP313 and currently(23.08.28) ATF has no support for it
# now it's showing error because RSB cannot init, possibly chip not found
# explictly disable it to prevent future breaking changes
# u-boot will handle the PMIC
ATF_EXTRA_PARAM += SUNXI_SETUP_REGULATORS=0

# U-Boot configs #
UBOOT_CODE_DIR := u-boot
UBOOT_BINARY = $(UBOOT_CODE_DIR)/u-boot-sunxi-with-spl.bin
UBOOT_DEFCONFIG := mangopi_mq_quad_defconfig
UBOOT_BUILD_PARAM :=
# UBOOT_BUILD_PARAM += ARCH=$(ARCH)
UBOOT_BUILD_PARAM += CROSS_COMPILE=$(CROSS_COMPILE)
UBOOT_BUILD_PARAM += BL31=$(abspath $(ATF_BINARY))

# Upstream u-boot does not support MQ Quad natively.
# even the hardware developer uses orange pi zero 2 as a template
# we will setup a separate device through patches
# we will use a full config instead of a saved defconfig
# UBOOT_DEFCONFIG := mangopi_mq_quad_defconfig

# Target config
PHONY := prepare-code-atf prepare-code-u-boot
PHONY += build-atf build-u-boot
PHONY += clean distclean

.PHONY: $(PHONY)

## Source code preparations

prepare-code-atf: $(ATF_CODE_DIR)
prepare-code-u-boot: $(UBOOT_CODE_DIR)

$(ATF_CODE_DIR):
	git clone --depth 1 https://github.com/ARM-software/arm-trusted-firmware.git $(ATF_CODE_DIR)

$(UBOOT_CODE_DIR):
	git clone --depth 1 https://github.com/u-boot/u-boot $(UBOOT_CODE_DIR)

## Source code patching


## Building

build-atf: $(ATF_BINARY)
build-u-boot: $(UBOOT_BINARY)

$(ATF_BINARY):
	$(MAKE) CROSS_COMPILE=aarch64-linux-gnu- PLAT=$(ATF_PLATFORM) $(ATF_EXTRA_PARAM) -C $(ATF_CODE_DIR) bl31

$(UBOOT_BINARY): $(ATF_BINARY)
	$(MAKE) -C $(UBOOT_CODE_DIR) $(UBOOT_BUILD_PARAM) $(UBOOT_DEFCONFIG)
	$(MAKE) -C $(UBOOT_CODE_DIR) $(UBOOT_BUILD_PARAM)

## All in one



## Clean

clean:
	[ ! -d $(ATF_CODE_DIR) ] || $(MAKE) -C $(ATF_CODE_DIR) clean
	[ ! -d $(UBOOT_CODE_DIR) ] || $(MAKE) -C $(UBOOT_CODE_DIR) mrproper

distclean:
	rm -rf $(ATF_CODE_DIR)
	rm -rf $(UBOOT_CODE_DIR)

## Debug

print-settings:
	@echo ATF_BINARY=$(ATF_BINARY)
	@echo ATF_EXTRA_PARAM=$(ATF_EXTRA_PARAM)
	@echo UBOOT_BINARY=$(UBOOT_BINARY)
	@echo UBOOT_BUILD_PARAM=$(UBOOT_BUILD_PARAM)
