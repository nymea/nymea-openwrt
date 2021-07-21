#
# Copyright (C) 2021 nymea GmbH <developer@nymea.io>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nymea-zigbee
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/nymea/nymea-zigbee.git
PKG_SOURCE_VERSION:=master

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
-include $(STAGING_DIR)/host/mk/qmake.mk

define Package/libnymea-zigbee
  SECTION:=libs
  CATEGORY:=nymea
  SUBMENU:=nymea-zigbee
  TITLE:=zigbee library for nymea
  URL:=https://github.com/nymea/nymea-zigbee
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+qt5-gui +qt5-sql +qt5-serialport +libudev
endef

define Package/libnymea-zigbee/description
  ZigBee library for nymea
endef

define Build/InstallDev
        $(MAKE) -C $(PKG_BUILD_DIR) install
endef

define Package/libnymea-zigbee/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/libnymea-zigbee/ $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libnymea-zigbee))
