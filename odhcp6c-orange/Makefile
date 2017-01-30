#
# Copyright (C) 2012-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=odhcp6co
PKG_VERSION:=2017-01-30
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/dGs-/odhcp6c.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=04bf5ae975596da1b6aa8800d9dd91e87411acfc
PKG_MAINTAINER:=Jerome LACHAISE <none@none>
PKG_LICENSE:=GPL-2.0

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

ifneq ($(CONFIG_PACKAGE_odhcp6c_ext_cer_id),0)
  CMAKE_OPTIONS += -DEXT_CER_ID=$(CONFIG_PACKAGE_odhcp6c_ext_cer_id)
endif

define Package/odhcp6c-orange
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Embedded DHCPv6-client for OpenWrt for Orange France
  DEPENDS:=@IPV6
endef

define Package/odhcp6co/postinst
#!/bin/sh
set -ex
[ -n "$$IPKG_INSTROOT" ] || {
killall -9 odhcp6c || true
}
endef

define Package/odhcp6co/config
  config PACKAGE_odhcp6c_ext_cer_id
    int "CER-ID Extension ID (0 = disabled)"
    depends on PACKAGE_odhcp6c
    default 0
endef

define Package/odhcp6co/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/odhcp6c $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_BIN) ./files/dhcpv6.sh $(1)/lib/netifd/proto/dhcpv6.sh
	$(INSTALL_BIN) ./files/dhcpv6.script $(1)/lib/netifd/
endef

$(eval $(call BuildPackage,odhcp6co))
