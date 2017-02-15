include $(TOPDIR)/rules.mk

PKG_NAME:=luvi
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/luvit/luvi.git
PKG_SOURCE_VERSION:=2.12.1
PKG_SOURCE_DATE:=2017-01-24

PKG_LICENSE:=Apache-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=luajit/host
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/luvi
 SUBMENU:=Lua
 SECTION:=lang
 CATEGORY:=Languages
 TITLE:=Luvi
 URL:=https://github.com/luvit/luvi
 MAINTAINER:=Morteza Milani <milani@pichak.co>
 DEPENDS:=+luajit +luv +libpcre +libopenssl +zlib +libuv
endef

define Package/luvi/description
 Luvi is a project to work with bundled luvit applications
endef

ifeq ($(CONFIG_i386),y)
LUVI_TARGET_ARCH = x86
else ifeq ($(CONFIG_x86_64),y)
LUVI_TARGET_ARCH = x64
else ifeq ($(CONFIG_powerpc),y)
LUVI_TARGET_ARCH = ppc
else ifeq ($(CONFIG_arm)$(CONFIG_armeb),y)
LUVI_TARGET_ARCH = arm
else ifeq ($(CONFIG_mips),y)
LUVI_TARGET_ARCH = mips
else ifeq ($(CONFIG_mipsel),y)
LUVI_TARGET_ARCH = mipsel
else
LUVI_TARGET_ARCH = $(CONFIG_ARCH)
endif

CMAKE_OPTIONS += -DBUILD_SHARED_LIBS=ON -DWithSharedLibluv=ON -DTARGET_ARCH=$(LUVI_TARGET_ARCH) -DLUA_PATH=$(STAGING_DIR)/host/share/luajit-2.1.0-beta2/?.lua -DLUAJIT_INCLUDE_DIR=$(STAGING_DIR)/usr/include/luajit-2.1 -DLUAJIT_LIBRARIES=$(STAGING_DIR)/usr/lib/libluajit-5.1.so

CMAKE_OPTIONS += -DWithPCRE=ON -DWithSharedPCRE=ON -DWithOpenSSL=ON -DWithOpenSSLASM=ON -DWithSharedOpenSSL=ON -DWithZLIB=ON -DWithSharedZLIB=ON

CMAKE_FIND_ROOT_PATH +=;$(STAGING_DIR)/host/bin

define Package/luvi/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/*.so $(1)/usr/lib/
	$(CP) $(STAGING_DIR)/usr/lib/libluajit*.so* $(1)/usr/lib
	$(LN) libluajit-5.1.so.2.1.0 $(1)/usr/lib/libluajit-5.1.so.2
endef

$(eval $(call BuildPackage,luvi))

