#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=x264
PKG_VERSION:=2019-11-01
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_VERSION:=7817004df0bf57e1eb83e8ef9c0c407477b59d71
PKG_SOURCE_URL_FILE:=$(PKG_SOURCE_VERSION).tar.bz2
PKG_SOURCE_URL:=https://code.videolan.org/videolan/x264/-/archive/
PKG_HASH:=ab65a42caae256c66f3edc4df060886b5ab2589d087a2e43d4a4df76c55e846a

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

TAR_OPTIONS:=--strip-components 1 $(TAR_OPTIONS)
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

TARGET_CFLAGS:=$(filter-out -O%,$(TARGET_CFLAGS))
MAKE_FLAGS:=$(filter-out LD=%,$(MAKE_FLAGS)) LD="$(TARGET_CC) -o"

# Instead of blacklisting a boatload of platforms just enable
# optimization where it makes sense (matters) ie ARMv7+ and x86_64

ifneq ($(findstring cortex-a,$(CPU_TYPE)),)
  CONFIGURE_ARGS += --enable-lto
  else
  ifneq ($(CONFIG_TARGET_x86_64),)
    CONFIGURE_ARGS += --enable-lto
    ifeq ($(CONFIG_NASM),y)
      CONFIGURE_VARS+= AS=nasm
      MAKE_FLAGS+= AS=nasm
    endif
    else
    CONFIGURE_VARS+= AS=
    MAKE_FLAGS+= AS=
    CONFIGURE_ARGS += --disable-asm
  endif
endif

CONFIGURE_ARGS += \
		--disable-cli \
		--enable-shared \
		--disable-opencl \
		--enable-pic \
		--disable-avs \
		--disable-ffms \
		--disable-gpac \
		--disable-lsmash

define Package/libx264
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=H264/AVC free codec library
  DEPENDS:=+libpthread @BUILD_PATENTED
  URL:=https://www.videolan.org/developers/x264.html
endef

define Package/libx264/description
  x264 is a free software library for encoding
  video streams into the H.264/MPEG-4 AVC compression format.
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/x264/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* $(1)/usr/lib/pkgconfig/
endef

define Package/libx264/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libx264))
