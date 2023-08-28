## Linux building script for MangoPi MQ-Quad

# kernel source configurations
KERNEL_VERSION := 6.5
_KERNEL_VERSION_MAJOR = $(firstword $(subst ., ,$(KERNEL_VERSION)))
_KERNEL_TARBALL_SAVED = linux-$(KERNEL_VERSION).tar.xz
_KERNEL_TARBALL_LINK := https://cdn.kernel.org/pub/linux/kernel/v$(_KERNEL_VERSION_MAJOR).x/linux-$(KERNEL_VERSION).tar.xz

KERNEL_SRC_DIR := linux-src-$(KERNEL_VERSION)

# Source code preparation #

prepare-code-linux: $(KERNEL_SRC_DIR)

$(KERNEL_SRC_DIR): $(_KERNEL_TARBALL_SAVED)
	mkdir $(KERNEL_SRC_DIR)
	tar xf $(_KERNEL_TARBALL_SAVED) --strip-components=1 -C $(KERNEL_SRC_DIR)

$(_KERNEL_TARBALL_SAVED):
	[ ! -f $(_KERNEL_TARBALL_SAVED).partial ] || rm -f $(_KERNEL_TARBALL_SAVED).partial
	curl -fL -o "$(_KERNEL_TARBALL_SAVED).partial" "$(_KERNEL_TARBALL_LINK)"
	mv "$(_KERNEL_TARBALL_SAVED).partial" $(_KERNEL_TARBALL_SAVED)

# Patching kernel source #

# Building kernel binaries #

# All in one #

# Debugging #
