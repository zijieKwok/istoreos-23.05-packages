#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=rkmpp
PKG_VERSION:=2022-11-07
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_VERSION:=ee6f5b3374e45fc4b1689ea8230f445350d4634d
PKG_SOURCE_URL_FILE:=$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/JeffyCN/mirrors/archive/
PKG_HASH:=5e6d2b10043f2f8680d37a74655d2ef25decdc483d7e042846351e5fefbc3326

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

TAR_OPTIONS:=--strip-components 1 $(TAR_OPTIONS)
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

define Package/rkmpp
  SECTION:=multimedia
  CATEGORY:=Multimedia
  TITLE:=Rockchip MPP
  DEPENDS:=@aarch64 +libstdcpp
  URL:=https://github.com/JeffyCN/mirrors/tree/mpp-dev
endef

define Package/rkmpp/description
 Rockchip Media Process Platform (MPP)
endef

define Package/rkmpp-demo
$(call Package/rkmpp)
  TITLE+= Demo
  DEPENDS:=+rkmpp
endef

define Package/rkmpp-demo/description
$(call Package/rkmpp/description)

This package provides the demo.
endef

CMAKE_OPTIONS += -DCMAKE_BUILD_TYPE=Release \
	-DHAVE_DRM=ON

define Package/rkmpp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib*.so.* $(1)/usr/lib/
endef

define Package/rkmpp-demo/install
	$(INSTALL_DIR) $(1)/usr
	$(CP) $(PKG_INSTALL_DIR)/usr/bin $(1)/usr/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/rockchip $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib $(1)/usr/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc $(1)/usr/lib/pkgconfig/
endef

$(eval $(call BuildPackage,rkmpp))
$(eval $(call BuildPackage,rkmpp-demo))
