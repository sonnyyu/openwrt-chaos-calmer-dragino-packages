include $(TOPDIR)/rules.mk

PKG_NAME:=foxconn-bus-lan
PKG_VERSION:=1.2.4

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)    
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=$(PKG_NAME)
	MAINTAINER:=<support@dragino.com>
endef

define Package/$(PKG_NAME)/description
	Package for Foxconn Bus LAN project
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	mkdir -p $(PKG_BUILD_DIR)/usr/bin
	mkdir -p $(PKG_BUILD_DIR)/usr/lib/lua/dragino
	$(CP) ./files/* $(PKG_BUILD_DIR)/
	$(CP) ./luasrc/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/update_status_foxconn $(PKG_BUILD_DIR)/update_status_foxconn.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/get_latest_foxconn_firmware $(PKG_BUILD_DIR)/get_latest_foxconn_firmware.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/QuaiKe.sock $(PKG_BUILD_DIR)/QuaiKe.sock.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/check_client $(PKG_BUILD_DIR)/check_client.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/send_AT_command $(PKG_BUILD_DIR)/send_AT_command.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/update_usb_modem_info $(PKG_BUILD_DIR)/update_usb_modem_info.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/gps $(PKG_BUILD_DIR)/gps.lua
	$(STAGING_DIR_HOST)/bin/luac -o $(PKG_BUILD_DIR)/process_AT_feedback $(PKG_BUILD_DIR)/process_AT_feedback.lua
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin	
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/update_status_foxconn $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/get_latest_foxconn_firmware $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/QuaiKe.sock $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/check_client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/send_AT_command $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/update_usb_modem_info $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/gps $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/process_AT_feedback $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/usr/bin/* $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc
	$(CP) $(PKG_BUILD_DIR)/etc/* $(1)/etc

	$(INSTALL_DIR) $(1)/usr/lib/lua/dragino/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/foxconn_server.lua $(1)/usr/lib/lua/dragino/
	
	$(INSTALL_DIR) $(1)/usr/lib/lua/
	$(CP) $(PKG_BUILD_DIR)/usr/lib/lua/* $(1)/usr/lib/lua/ 
	
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	/usr/bin/postinst-foxconn-bus-lan
fi
exit 0
endef

$(eval $(call BuildPackage,$(PKG_NAME)))