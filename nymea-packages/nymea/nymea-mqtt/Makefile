#
# Copyright (C) 2021 nymea GmbH <developer@nymea.io>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nymea-mqtt
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/nymea/nymea-mqtt.git
PKG_SOURCE_VERSION:=master

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
-include $(STAGING_DIR)/host/mk/qmake.mk

define Package/libnymea-mqtt
  SECTION:=libs
  CATEGORY:=nymea
  SUBMENU:=nymea-mqtt
  TITLE:=MQTT library for nymea
  URL:=https://github.com/nymea/nymea-mqtt
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+qt5-network
endef

define Package/libnymea-mqtt/description
  MQTT library for nymea
endef


define Package/nymea-mqttserver
  SECTION:=utils
  CATEGORY:=nymea
  TITLE:=Standalone MQTT server
  SUBMENU:=nymea-mqtt
  URL:=https://github.com/nymea/nymea-mqtt
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+libnymea-mqtt
endef

define Package/nymea-mqttserver/description
  Standalone MQTT server based on nymea-mqtt
endef

define Build/InstallDev
        $(MAKE) -C $(PKG_BUILD_DIR) install
endef

define Package/libnymea-mqtt/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libnymea-mqtt/*.so* $(1)/usr/lib/
endef

define Package/nymea-mqttserver/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/server/nymea-mqttserver $(1)/usr/bin/
endef

$(eval $(call BuildPackage,libnymea-mqtt))
$(eval $(call BuildPackage,nymea-mqttserver))
