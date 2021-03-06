# You may redistribute this program and/or modify it under the terms of
# the GNU General Public License as published by the Free Software Foundation,
# either version 3 of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
include $(TOPDIR)/rules.mk

PKG_NAME:=cjdns
PKG_VERSION:=master

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=git://github.com/cjdelisle/cjdns.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_DEPENDS:=nodejs


include $(INCLUDE_DIR)/package.mk

define Package/cjdns
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Routing and Redirection
  TITLE:=Experimental self configuring routing protocol.
  DEPENDS:=+kmod-tun +kmod-ipv6 +libnl +libpthread +librt
  MAINTAINER:=cjd -- #cjdns on irc.efnet.org
endef

define cjdns/conffiles
  /etc/cjdroute.conf
endef

define Build/Configure
	$(CP) plans/mipso32_plan.json $(PKG_BUILD_DIR)/node_build/dependencies/cnacl/node_build/plans
endef

define Build/Compile
	( cd $(PKG_BUILD_DIR); CROSS_COMPILE=$(TARGET_CROSS) $(PKG_BUILD_DIR)/cross-do )
endef

# Need to tell cjdns build process that there exists an nacl on the system.
CMAKE_OPTIONS += -DHAVE_NACL=1

define Package/cjdns/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cjdroute   $(1)/usr/sbin/cjdroute
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/cjdns.init $(1)/etc/init.d/cjdns
endef

$(eval $(call BuildPackage,cjdns))
