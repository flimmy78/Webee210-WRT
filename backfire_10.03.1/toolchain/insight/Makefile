# 
# Copyright (C) 2006-2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=insight
PKG_VERSION:=6.8-1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_MD5SUM:=4ee9824c1e8d6108d886c6c09b24f0ac
PKG_SOURCE_URL:=ftp://sourceware.org/pub/insight/releases
PKG_CAT:=bzcat

STAGING_DIR_HOST:=$(TOOLCHAIN_DIR)
BUILD_DIR_HOST:=$(BUILD_DIR_TOOLCHAIN)

include $(INCLUDE_DIR)/host-build.mk

define Host/Configure
	(cd $(HOST_BUILD_DIR); \
		gdb_cv_func_sigsetjmp=yes \
		CFLAGS="-O2" \
		$(HOST_BUILD_DIR)/configure \
		--prefix=$(TOOLCHAIN_DIR)/usr \
		--build=$(GNU_HOST_NAME) \
		--host=$(GNU_HOST_NAME) \
		--target=$(REAL_GNU_TARGET_NAME) \
		$(DISABLE_NLS) \
		--enable-threads \
		--enable-werror=no \
	);
endef

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR)
endef

define Host/Install
	mkdir -p $(TOOLCHAIN_DIR)/usr/bin
	$(INSTALL_BIN) $(HOST_BUILD_DIR)/gdb/insight $(TOOLCHAIN_DIR)/usr/bin/$(TARGET_CROSS)insight
	ln -fs $(TARGET_CROSS)insight $(TOOLCHAIN_DIR)/usr/bin/$(GNU_TARGET_NAME)-insight
	strip $(TOOLCHAIN_DIR)/usr/bin/$(TARGET_CROSS)insight
endef

define Host/Clean
	rm -rf \
		$(HOST_BUILD_DIR) \
		$(TOOLCHAIN_DIR)/usr/bin/$(TARGET_CROSS)insight \
		$(TOOLCHAIN_DIR)/usr/bin/$(GNU_TARGET_NAME)-insight
endef

$(eval $(call HostBuild))
