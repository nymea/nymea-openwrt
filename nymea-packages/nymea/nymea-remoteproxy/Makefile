#
# Copyright (C) 2021 nymea GmbH <developer@nymea.io>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=nymea-remoteproxy
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/nymea/nymea-remoteproxy.git
PKG_SOURCE_VERSION:=master

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
-include $(STAGING_DIR)/host/mk/qmake.mk

define Package/libnymea-remoteproxy
  SECTION:=libs
  CATEGORY:=nymea
  SUBMENU:=nymea-remoteproxy
  TITLE:=Remote proxy connection library for nymea
  URL:=https://github.com/nymea/nymea-remoteproxy
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+qt5-network +qt5-websockets
endef

define Package/libnymea-remoteproxy/description
  Remote proxy connection library for nymea
endef

define Package/libnymea-remoteproxyclient
  SECTION:=utils
  CATEGORY:=nymea
  TITLE:=Remote proxy connection client library for nymea
  SUBMENU:=nymea-remoteproxy
  URL:=https://github.com/nymea/nymea-remoteproxy
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+libnymea-remoteproxy
endef

define Package/libnymea-remoteproxyclient/description
  Remote proxy connection client library for nymea.
endef

define Package/nymea-remoteproxy-client
  SECTION:=utils
  CATEGORY:=nymea
  TITLE:=Standalone nymea remote proxy connection client
  SUBMENU:=nymea-remoteproxy
  URL:=https://github.com/nymea/nymea-remoteproxy
  MAINTAINER:=nymea GmbH <developer@nymea.io>
  DEPENDS+=+libnymea-remoteproxyclient
endef

define Package/nymea-remoteproxy-client/description
  Standalone nymea remote proxy connection client.
endef

define Build/InstallDev
        $(MAKE) -C $(PKG_BUILD_DIR) install
endef

define Package/libnymea-remoteproxy/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libnymea-remoteproxy/*.so* $(1)/usr/lib/
endef

define Package/libnymea-remoteproxyclient/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libnymea-remoteproxyclient/*.so* $(1)/usr/lib/
endef

define Package/nymea-remoteproxy-client/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(CP) $(PKG_BUILD_DIR)/client/nymea-remoteproxy-client $(1)/usr/bin/
endef


$(eval $(call BuildPackage,libnymea-remoteproxy))
$(eval $(call BuildPackage,libnymea-remoteproxyclient))
$(eval $(call BuildPackage,nymea-remoteproxy-client))
