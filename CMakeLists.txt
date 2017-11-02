cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(limituj-czas LANGUAGES NONE)

add_subdirectory(po)

install(FILES ${PROJECT_NAME}.cfg DESTINATION /etc)
install(FILES czasy.log DESTINATION /var/lib/${PROJECT_NAME})
install(FILES ${PROJECT_NAME} DESTINATION sbin)
install(FILES zliczaj-czas.html copyright README.md changelog DESTINATION share/doc/${PROJECT_NAME})

set(CPACK_GENERATOR DEB)
set(CPACK_SET_DESTDIR ON)
set(CPACK_PACKAGING_INSTALL_PREFIX /usr) # not working?
set(CMAKE_INSTALL_PREFIX /usr)
set(CPACK_INCLUDE_TOPLEVEL_DIRECTORY OFF)

set(CPACK_PACKAGE_VERSION_MAJOR 0)
set(CPACK_PACKAGE_VERSION_MINOR 1)
set(CPACK_PACKAGE_VERSION_PATCH 0)
set(CPACK_PACKAGE_VENDOR slodki)
set(CPACK_PACKAGE_CONTACT "Tomasz Słodkowicz <slodki@users.noreply.github.com>")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "User working time limit tool")
set(CPACK_PACKAGE_CHECKSUM SHA256)

set(CPACK_DEBIAN_PACKAGE_SECTION admin)
set(CPACK_DEBIAN_PACKAGE_PRIORITY extra)
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE https://github.com/slodki/limituj-czas)
set(CPACK_DEBIAN_PACKAGE_DEPENDS "bash, procps, systemd, kde-baseapps-bin, libkscreenlocker5")
set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE all)

include(CPack)
