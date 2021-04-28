# Linux kernel Makefile

override SRC_TOP:=..

XLINUX_DIR := linux-rpt
NUM_CPU := $(shell grep -c ^processor /proc/cpuinfo)
INSTALL_DIR ?= .

KIMAGE=$(XLINUX_DIR)/arch/arm64/boot/Image.gz
#CONFIG=xilinx_zynqmp_defconfig
CONFIG=xilinx_u96v1_defconfig

# Versioning system
BUILD_NUMBER ?= 1
REVISION ?= devbuild
VER ?= 00.00
VERSION=$(VER)-$(BUILD_NUMBER)

export BUILD_NUMBER
export REVISION
export VERSION

# Driver debug build flags
#DBGKERNEL_CFLAGS = -O0 -g -DDEBUG=2
#DBGDRIVER_CFLAGS = -O0 -g -DDEBUG=2
#export DBGKERNEL_CFLAGS
#export DBGDRIVER_CFLAGS

.PHONY : all install clean distclean

all: $(KIMAGE)

$(KIMAGE): | $(XLINUX_DIR)
	echo $(shell echo $(BUILD_NUMBER) - 1 | bc ) > $(XLINUX_DIR)/.version
	make -C $(XLINUX_DIR) mrproper
	make -C $(XLINUX_DIR) ARCH=arm64 $(CONFIG)
	make -j $$(($(NUM_CPU)+1)) -C $(XLINUX_DIR) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-
	make -j $$(($(NUM_CPU)+1)) -C $(XLINUX_DIR) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- Image
	make -j $$(($(NUM_CPU)+1)) -C $(XLINUX_DIR) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- modules
	make -C $(XLINUX_DIR) ARCH=arm64 COMPILED_SOURCE=1 cscope
#	make -C $(XLINUX_DIR) ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_PATH=../linux-modules/ modules_install
#	cd $(XLINUX_DIR); fakeroot make-kpkg --arch arm64 --cross-compile aarch64-linux-gnu- kernel_headers

clean:
	-make -C $(XLINUX_DIR) ARCH=arm64 clean
	-make -C $(XLINUX_DIR) mrproper
	-rm -f *.deb
	-rm -rf $(XLINUX_DIR)/oe-logs
	-rm -rf $(XLINUX_DIR)/oe-workdir

install: $(KIMAGE)
	cp $(KIMAGE) $(INSTALL_DIR)
#	dpkg -x *.deb $(USR_ROOT)

distclean: clean
	-make -C $(XLINUX_DIR) mrproper

