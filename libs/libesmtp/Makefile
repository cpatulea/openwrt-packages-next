#
# Copyright (C) 2008-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libesmtp
PKG_VERSION:=1.0.6
PKG_RELEASE:=1
PKG_MD5SUM:=c4fedc999b6c3820296b0eb92cc2e252

PKG_MAINTAINER:=Othmar Truniger <github@truniger.ch>
PKG_LICENSE:=LGPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE_URL:=http://www.stafford.uklinux.net/libesmtp
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_CAT:=zcat

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/ipkg-install

include $(INCLUDE_DIR)/package.mk

define Package/libesmtp
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=A Library for Posting Electronic Mail
  URL:=http://www.stafford.uklinux.net/libesmtp/
  DEPENDS:=+libpthread
endef

define Build/Configure
	$(call Build/Configure/Default,--without-openssl)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Build/InstallDev
	mkdir -p $(STAGING_DIR)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libesmtp.h $(STAGING_DIR)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/auth-client.h $(STAGING_DIR)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/auth-plugin.h $(STAGING_DIR)/usr/include/
	mkdir -p $(STAGING_DIR)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libesmtp.{a,so*} $(STAGING_DIR)/usr/lib/
	$(INSTALL_DIR) $(2)/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/libesmtp-config $(2)/bin/
	$(SED) 's,^\(prefix\|exec_prefix\)=.*,\1=$(STAGING_DIR)/usr,g' $(2)/bin/libesmtp-config
endef

define Build/UninstallDev
	rm -rf \
		$(STAGING_DIR)/usr/include/libesmtp.h \
		$(STAGING_DIR)/usr/include/auth-client.h \
		$(STAGING_DIR)/usr/include/auth-plugin.h \
		$(STAGING_DIR)/usr/lib/libesmtp.{a,so*}
endef

define Package/libesmtp/install
	mkdir -p $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libesmtp.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libesmtp))
