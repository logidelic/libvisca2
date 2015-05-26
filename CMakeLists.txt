cmake_minimum_required(VERSION 2.6)

include(ReMake)

remake_project(
  libvisca2
  VERSION 2.0
  RELEASE beta
  SUMMARY "Driver library for controlling VISCA-compliant cameras via RS232."
  AUTHOR "Damien Douxchamps"
  AUTHOR "Mikael Rosbacke"
  AUTHOR "Joerg Desch"
  AUTHOR "Alexander Klaeser"
  CONTACT "ralf.kaestner@gmail.com"
  HOME "http://damien.douxchamps.net/libvisca"
  LICENSE "GNU Lesser General Public License (LGPL)"
  README README.md
  PREFIX visca-
  INSTALL /usr
  HEADER_DESTINATION include/libvisca
)

remake_component(
  udev
  CONFIGURATION_DESTINATION /etc/udev
)
remake_add_directories(
  rules
  COMPONENT udev
)
remake_doc(
  html
)

remake_pack_deb()
remake_pack_deb(
  COMPONENT dev
  DESCRIPTION "development headers"
  DEPENDS libvisca2
)
remake_pack_deb(
  COMPONENT utils
  DESCRIPTION "utilities"
  DEPENDS libvisca2
)
remake_pack_deb(
  COMPONENT udev
  DESCRIPTION "udev rules"
  DEPENDS udev
  EXTRA debian/rules/*
)
remake_pack_deb(
  ARCH all
  COMPONENT doc
  DESCRIPTION "documentation"
)

remake_distribute_deb(
  DISTRIBUTION precise
  SECTION libs
  UPLOAD ppa:kralf/asl
  DEPENDS remake doxygen pkg-config
  PASS CMAKE_BUILD_TYPE LIBVISCA2_GIT_REVISION
)
remake_distribute_deb(
  DISTRIBUTION trusty
  SECTION libs
  UPLOAD ppa:kralf/asl
  DEPENDS remake doxygen pkg-config
  PASS CMAKE_BUILD_TYPE LIBVISCA2_GIT_REVISION
)
