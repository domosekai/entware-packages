#
# Copyright (C) 2007-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ndppd
PKG_VERSION:=0.2.6
PKG_RELEASE:=3

# Latest release
#PKG_SOURCE_URL:=https://codeload.github.com/DanielAdolfsson/ndppd/tar.gz/$(PKG_VERSION)?
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_HASH:=ee934167f8357f0bd0015e201a77fbe4d028c59e89dc98113805c6855e1c3992
PKG_LICENSE:=GPL-3.0-or-later

# Development snapshot
PKG_SOURCE_URL:=https://github.com/domosekai/ndppd.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=bugfix
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/ndppd
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NDP Proxy Daemon
  URL:=http://www.priv.nu/projects/ndppd/
  MAINTAINER:=Gabriel Kerneis <gabriel@kerneis.info>
  DEPENDS:=@IPV6 +libstdcpp
endef

define Package/ndppd/description
 ndppd, or NDP Proxy Daemon, is a daemon that proxies NDP (Neighbor Discovery
 Protocol) messages between interfaces.  ndppd currently only supports Neighbor
 Solicitation Messages and Neighbor Advertisement Messages.

 The ndp_proxy provided by Linux doesn't support listing proxies, and only hosts
 are supported.  No subnets.  ndppd solves this by listening for Neighbor
 Solicitation messages on an interface, then query the internal interfaces for
 that target IP before finally sending a Neighbor Advertisement message.
endef

define Package/ndppd/conffiles
/opt/etc/ndppd.conf
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS) -std=c++0x -fno-rtti -Wno-narrowing" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		LIBS="-lc" \
		ndppd
endef

define Package/ndppd/install
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ndppd $(1)/opt/sbin/
	$(INSTALL_DIR) $(1)/opt/etc/init.d
	$(INSTALL_BIN) ./files/S33ndppd $(1)/opt/etc/init.d/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/ndppd.conf-dist $(1)/opt/etc/ndppd.conf
endef

$(eval $(call BuildPackage,ndppd))
