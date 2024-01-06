#
# Copyright (C) 2021 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tailscale

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/$(PKG_NAME/$(PKG_NAME).git
PKG_SOURCE_DATE:=2024-01-08
PKG_SOURCE_VERSION:=c05c4bdce410db14f742d61968945a9312a7a0de
PKG_MIRROR_HASH:=02d7814bfc072ffdd8482688e0d39086d31362cd951113b6c51516e2216402ee

PKG_MAINTAINER:=Jan Pavlinec <jan.pavlinec1@gmail.com>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host upx/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=\
	tailscale.com/cmd/tailscaled
GO_PKG_LDFLAGS:=-X 'tailscale.com/version.longStamp=$(PKG_VERSION)-$(PKG_RELEASE) (OpenWrt)'
GO_PKG_LDFLAGS_X:=tailscale.com/version.shortStamp=$(PKG_VERSION)
GO_PKG_TAGS:=ts_include_cli,ts_omit_aws,ts_omit_bird,ts_omit_tap,ts_omit_kube

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/tailscale
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=Zero config VPN
  URL:=https://tailscale.com
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle +kmod-tun
  PROVIDES:=tailscaled
endef

define Package/tailscale/description
  It creates a secure network between your servers, computers,
  and cloud instances. Even when separated by firewalls or subnets.
endef

define Package/tailscale/conffiles
/etc/config/tailscale
/etc/tailscale/
endef

define Package/tailscale/install
	$(INSTALL_DIR) $(1)/usr/sbin $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/tailscaled $(1)/usr/sbin
	$(INSTALL_BIN) ./files//tailscale.init $(1)/etc/init.d/tailscale
	$(INSTALL_DATA) ./files//tailscale.conf $(1)/etc/config/tailscale
	$(STRIP) $(1)/usr/sbin/tailscaled
	$(STAGING_DIR_HOST)/bin/upx --lzma --best $(1)/usr/sbin/tailscaled
	$(LN) tailscaled $(1)/usr/sbin/tailscale
endef

$(eval $(call BuildPackage,tailscale))
