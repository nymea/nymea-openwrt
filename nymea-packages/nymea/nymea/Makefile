#
# Copyright (C) 2021 nymea GmbH <developer@nymea.io>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nymea
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/nymea/nymea.git
PKG_SOURCE_VERSION:=build-openwrt

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
-include $(STAGING_DIR)/host/mk/qmake.mk

define Package/libnymea
  SECTION:=libs
  CATEGORY:=nymea
  SUBMENU:=nymea
  TITLE:=library for nymea
  URL:=https://github.com/nymea/nymea
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+qt5-network +qt5-dbus +qt5-serialbus +libnymea-zigbee +libnymea-mqtt
endef

define Package/libnymea/description
  library for nymea
endef

define Package/nymead
  SECTION:=utils
  CATEGORY:=nymea
  TITLE:=nymea server
  SUBMENU:=nymea
  URL:=https://github.com/nymea/nymea
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+libnymea +libnymea-remoteproxyclient +libnymea-zigbee +qt5-xml
endef

define Package/nymead/description
  nymea server
endef

define Build/Configure
  $(call Build/Configure/Default,nymea,CONFIG+=disabletesting)
endef

define Build/InstallDev
        $(MAKE) -C $(PKG_BUILD_DIR) install
endef

define Package/libnymea/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libnymea/*.so* $(1)/usr/lib/
endef

define Package/nymead/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libnymea-core/*.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/server/nymead $(1)/usr/bin/
endef

$(eval $(call BuildPackage,libnymea))
$(eval $(call BuildPackage,nymead))
