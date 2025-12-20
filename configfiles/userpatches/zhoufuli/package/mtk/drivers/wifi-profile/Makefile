#
# Copyright (C) 2016 MediaTek
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/version.mk

PKG_NAME:=wifi-profile
PKG_RELEASE:=1
PKG_LICENSE:=GPL-2.0

PKG_KCONFIG:= \
	first_card \
	first_card_name \
	second_card \
	second_card_name \
	third_card \
	third_card_name \
	WIFI_NORMAL_SETTING \
	WIFI_QUICK_SETTING \
	MTK_WIFI_SKU_TYPE

PKG_CONFIG_DEPENDS:=$(foreach c, $(PKG_KCONFIG),$(if $(CONFIG_$c),CONFIG_$(c)))

include $(INCLUDE_DIR)/package.mk

define Package/wifi-profile
  SECTION:=MTK Properties
  CATEGORY:=MTK Properties
  SUBMENU:=Drivers
  DEPENDS:=+wifi-dats
  TITLE:=MTK WiFi setup scripts
  VERSION:=$(PKG_RELEASE)
  MENU:=1
endef

define Package/wifi-profile/description
  This package install wifi setup scripts.
endef

define Package/wifi-profile/config
if PACKAGE_wifi-profile
	choice
		prompt "WiFi Setting"
		default WIFI_NORMAL_SETTING
		config WIFI_NORMAL_SETTING
			bool "normal setting"

		config WIFI_QUICK_SETTING
			bool "quick setting"

		help
			This is a way to configure wifi after wifi reload(Save & Apply from UI).
			Normal setting means reloading dat file which will execute "wifi down; wifi up"
			Quick setting means executing iwpriv commands corresponding to your operation on UI.
	endchoice
endif
endef

define Package/wifi-dats
  SECTION:=MTK Properties
  CATEGORY:=MTK Properties
  SUBMENU:=Drivers
  TITLE:=MTK WiFi l1 & l2 dat files
  VERSION:=$(PKG_RELEASE)
  MENU:=1
endef

define Package/wifi-dats/description
  This package install wifi l1 and l2 dat files.
endef

define Package/wifi-dats/config
if PACKAGE_wifi-dats
	menuconfig first_card
		bool "1st card"
		default y

	if first_card
	config first_card_name
		string "1st card name"
		default "MT7916"
	endif

endif
endef

define Package/wifi-dats/conffiles
/etc/wireless/
endef

define Build/Compile
endef

define Package/wifi-profile/install
	$(INSTALL_DIR) $(1)/lib/wifi/
	$(INSTALL_DIR) $(1)/sbin/

	$(INSTALL_BIN) ./files/common/wifi_jedi $(1)/sbin/wifi
	$(INSTALL_BIN) ./files/common/mtwifi.lua $(1)/lib/wifi/

	if [ "$$(CONFIG_WIFI_QUICK_SETTING)" = "y" ] ; then \
		$(INSTALL_BIN) ./files/common/quick_setting.lua $(1)/lib/wifi/; \
	fi
endef

define Package/wifi-dats/install
	$(INSTALL_DIR) $(1)/etc/wireless/mediatek/
	$(INSTALL_DATA) ./files/mt7916/* $(1)/etc/wireless/mediatek/;
	$(INSTALL_DATA) ./files/mt7916/l1profile.dat $(1)/etc/wireless/l1profile.dat;
endef

$(eval $(call BuildPackage,wifi-dats))
$(eval $(call BuildPackage,wifi-profile))
