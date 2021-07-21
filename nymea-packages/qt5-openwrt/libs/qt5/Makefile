#
# Copyright (C) 2013 Riccardo Ferrazzo <f.riccardo87@gmail.com>
# Copyright (C) 2017 Paweł Knioła <pawel.kn@gmail.com>
# Copyright (C) 2019 Yaroslav Petrov <info@lank.me>
# Copyright (C) 2021 nymea GmbH <developer@nymea.io>
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=qt5
PKG_VERSION:=5.12
PKG_RELEASE:=6

PKG_SOURCE:=qt-everywhere-src-$(PKG_VERSION).$(PKG_RELEASE).tar.xz
PKG_SOURCE_URL:=http://download.qt-project.org/archive/qt/$(PKG_VERSION)/$(PKG_VERSION).$(PKG_RELEASE)/single
PKG_MD5SUM:=287d71e71ebd97f77220873e7b131b1a

PKG_BUILD_DIR=$(BUILD_DIR)/qt-everywhere-src-$(PKG_VERSION).$(PKG_RELEASE)
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0
PKG_BUILD_DEPENDS:=libiconv

PKG_MAINTAINER:=nymea GmbH <developer@nymea.io>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

include ./files/qmake.mk

# We need to build the target qmake before we can build the host qmake
HOST_BUILD_DEPENDS:=qt5

# not using sstrip here as this fucks up the .so's somehow
STRIP:=/bin/true
RSTRIP:= \
  NM="$(TOOLCHAIN_DIR)/bin/$(TARGET_CROSS)nm" \
  STRIP="$(STRIP)" \
  STRIP_KMOD="$(STRIP)" \
  $(SCRIPT_DIR)/rstrip.sh


define Package/qt5/Default
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=Qt5
  TITLE:=qt5
  URL:=http://qt-project.org
  DEPENDS:=+librt +zlib +libzstd +libstdcpp +libpcre2-16 +libpthread +libatomic +libdbus $(ICONV_DEPENDS) +PACKAGE_icu:icu
endef

define Package/qt5-core
  $(call Package/qt5/Default)
  TITLE+=core
endef

define Package/qt5-concurrent
  $(call Package/qt5/Default)
  TITLE+=concurrent
  DEPENDS+=+qt5-core
endef

define Package/qt5-network
  $(call Package/qt5/Default)
  TITLE+=network
  DEPENDS+=+qt5-core +libopenssl
endef

define Package/qt5-dbus
  $(call Package/qt5/Default)
  TITLE+=DBus
  DEPENDS+=+qt5-core +libdbus
endef

define Package/qt5-gui
  $(call Package/qt5/Default)
  TITLE+=gui
  DEPENDS+=+qt5-core +qt5-network +qt5-dbus +libjpeg +libudev +mtdev +libinput
endef

define Package/qt5-widgets
  $(call Package/qt5/Default)
  TITLE+=widgets
  DEPENDS+=+qt5-core +qt5-gui +qt5-network +qt5-dbus
endef

define Package/qt5-sql
  $(call Package/qt5/Default)
  TITLE+=sql
  DEPENDS+=+qt5-core
endef

define Package/qt5-xml
  $(call Package/qt5/Default)
  TITLE+=xml
  DEPENDS+=+qt5-core
endef

define Package/qt5-serialport
  $(call Package/qt5/Default)
  TITLE+=serialport
  DEPENDS+=+qt5-core +libudev
endef

define Package/qt5-serialbus
  $(call Package/qt5/Default)
  TITLE+=serialbus
  DEPENDS+=+qt5-core +qt5-network +qt5-serialport
endef

define Package/qt5-websockets
  $(call Package/qt5/Default)
  TITLE+=websockets
  DEPENDS+=+qt5-core +qt5-network
endef

define Package/qt5-xmlpatterns
  $(call Package/qt5/Default)
  TITLE+=xmlpatterns
  DEPENDS+=+qt5-core +qt5-network
endef

define Package/qt5-test
  $(call Package/qt5/Default)
  TITLE+=test
  DEPENDS+=+qt5-core
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/qtbase/lib/fonts
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++
	$(CP) ./files/fonts/* $(PKG_BUILD_DIR)/qtbase/lib/fonts/
	$(CP) ./files/qplatformdefs.h $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++/qplatformdefs.h
	$(CP) ./files/qmake.conf $(PKG_BUILD_DIR)/qtbase/mkspecs/linux-openwrt-g++/qmake.conf
endef

define Build/Configure
	( cd $(PKG_BUILD_DIR) ; \
                TARGET_CROSS="$(TARGET_CROSS)" \
                TARGET_CFLAGS="$(TARGET_CFLAGS)" \
                TARGET_CXXFLAGS="$(TARGET_CFLAGS) $(TARGET_CXXFLAGS)" \
                TARGET_LDFLAGS="$(TARGET_LDFLAGS)" \
                PKG_CONFIG_SYSROOT_DIR="$(STAGING_DIR)" \
                ./configure --help ; \
		\
		TARGET_CROSS="$(TARGET_CROSS)" \
		TARGET_CFLAGS="$(TARGET_CFLAGS)" \
		TARGET_CXXFLAGS="$(TARGET_CFLAGS) $(TARGET_CXXFLAGS)" \
		TARGET_LDFLAGS="$(TARGET_LDFLAGS)" \
		PKG_CONFIG_SYSROOT_DIR="$(STAGING_DIR)" \
		./configure \
			-prefix $(QT_INSTALL_PREFIX) \
			-extprefix $(QT_EXTPREFIX) \
			-hostprefix $(QT_HOST_PREFIX) \
			-bindir $(QT_INSTALL_BINS) \
			-headerdir $(QT_INSTALL_HEADERS) \
			-libdir $(QT_INSTALL_LIBS) \
			-archdatadir $(QT_INSTALL_ARCHDATA) \
			-plugindir $(QT_INSTALL_PLUGINS) \
			-libexecdir $(QT_INSTALL_LIBEXECS) \
			-importdir $(QT_INSTALL_IMPORTS) \
			-qmldir $(QT_INSTALL_QML) \
			-datadir $(QT_INSTALL_DATA) \
			-docdir $(QT_INSTALL_DOCS) \
			-translationdir $(QT_INSTALL_TRANSLATIONS) \
			-sysconfdir $(QT_INSTALL_CONFIGURATION) \
			-examplesdir $(QT_INSTALL_EXAMPLES) \
			-testsdir $(QT_INSTALL_TESTS) \
			-hostbindir $(QT_HOST_BINS) \
			-hostlibdir $(QT_HOST_LIBS) \
			-hostdatadir $(QT_HOST_DATA) \
			-xplatform linux-openwrt-g++ \
			-opensource \
			-confirm-license \
			-force-asserts \
			-no-iconv \
			-no-pch \
			-no-rpath \
			-no-strip \
			-no-cups \
			-no-eglfs \
			-no-kms \
			-no-opengl \
			-no-directfb \
			-no-xcb \
			--openssl-linked \
			-qt-zlib \
			-qt-freetype \
			-make libs \
			-nomake examples \
			-nomake tests \
			-skip qtdeclarative \
			-skip qtmultimedia \
			-skip activeqt \
			-skip qtdoc \
			-skip qtconnectivity \
			-skip wayland \
			-skip qtscript \
			-L$(STAGING_DIR)/usr/lib \
			-v \
	)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR)/host/mk
	$(CP) ./files/qmake.mk $(STAGING_DIR)/host/mk/

	$(MAKE) -C $(PKG_BUILD_DIR) install
	$(CP) $(PKG_BUILD_DIR)/qtbase/bin/qmake $(TOOLCHAIN_DIR)/bin/
	$(CP) $(PKG_BUILD_DIR)/qtbase/bin/qt.conf $(TOOLCHAIN_DIR)/bin/
endef

define Package/qt5-core/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Core.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Core.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Core.la $(1)/usr/lib/
endef

define Package/qt5-concurrent/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Concurrent.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Concurrent.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Concurrent.la $(1)/usr/lib/
endef

define Package/qt5-network/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Network.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Network.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Network.la $(1)/usr/lib/
endef

define Package/qt5-dbus/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5DBus.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5DBus.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5DBus.la $(1)/usr/lib/
endef

define Package/qt5-gui/install
        $(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/generic/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/platforms/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/imageformats/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Gui.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Gui.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Gui.la 	$(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/generic/*.so $(1)/usr/lib/Qt/plugins/generic/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/platforms/*.so $(1)/usr/lib/Qt/plugins/platforms/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/imageformats/*.so $(1)/usr/lib/Qt/plugins/imageformats/
endef


define Package/qt5-widgets/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/fonts/
	$(CP) $(PKG_BUILD_DIR)/qtsvg/lib/libQt5Svg.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtsvg/lib/libQt5Svg.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtsvg/lib/libQt5Svg.la $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Widgets.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Widgets.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Widgets.la $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/fonts/* $(1)/usr/lib/fonts/
endef

define Package/qt5-sql/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/sqldrivers/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Sql.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Sql.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Sql.la 	$(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/plugins/sqldrivers/*.so $(1)/usr/lib/Qt/plugins/sqldrivers/
endef

define Package/qt5-xml/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Xml.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Xml.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Xml.la 	$(1)/usr/lib/
endef

define Package/qt5-serialport/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialport/lib/libQt5SerialPort.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialport/lib/libQt5SerialPort.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialport/lib/libQt5SerialPort.la $(1)/usr/lib/
endef

define Package/qt5-serialbus/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/Qt/plugins/canbus/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/lib/libQt5SerialBus.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/lib/libQt5SerialBus.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/lib/libQt5SerialBus.la $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtserialbus/plugins/canbus/*.so $(1)/usr/lib/Qt/plugins/canbus/
endef

define Package/qt5-websockets/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtwebsockets/lib/libQt5WebSockets.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtwebsockets/lib/libQt5WebSockets.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtwebsockets/lib/libQt5WebSockets.la $(1)/usr/lib/
endef

define Package/qt5-xmlpatterns/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtxmlpatterns/lib/libQt5XmlPatterns.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtxmlpatterns/lib/libQt5XmlPatterns.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtxmlpatterns/lib/libQt5XmlPatterns.la $(1)/usr/lib/
endef

define Package/qt5-test/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Test.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Test.prl $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/qtbase/lib/libQt5Test.la $(1)/usr/lib/
endef

$(eval $(call BuildPackage,qt5-core))
$(eval $(call BuildPackage,qt5-concurrent))
$(eval $(call BuildPackage,qt5-network))
$(eval $(call BuildPackage,qt5-dbus))
$(eval $(call BuildPackage,qt5-gui))
$(eval $(call BuildPackage,qt5-widgets))
$(eval $(call BuildPackage,qt5-sql))
$(eval $(call BuildPackage,qt5-xml))
$(eval $(call BuildPackage,qt5-serialport))
$(eval $(call BuildPackage,qt5-serialbus))
$(eval $(call BuildPackage,qt5-websockets))
$(eval $(call BuildPackage,qt5-xmlpatterns))
$(eval $(call BuildPackage,qt5-test))
