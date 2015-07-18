cmake_minimum_required(VERSION 2.8.0)
project(lcm)

include(ExternalProject)

find_program(install_program install)
if (NOT install_program)
	message(FATAL_ERROR "could not find install")
endif()
string(STRIP ${install_program} install_program)

if (${CMAKE_BUILD_TYPE} MATCHES Debug)
	set(OPT_FLAGS "-g")
endif()

ExternalProject_Add(lcm
  URL https://github.com/lcm-proj/lcm/releases/download/v1.2.1/lcm-1.2.1.zip
  URL_HASH MD5=4355889a2367697a9458e83f6251f2b2
  BUILD_IN_SOURCE 1
  CONFIGURE_COMMAND ./configure "--prefix=${CMAKE_INSTALL_PREFIX}" "INSTALL=${install_program} -c -C"
#		PKG_CONFIG_PATH=\"$ENV{PKG_CONFIG_PATH}:${CMAKE_INSTALL_PREFIX}/lib/pkgconfig\" 
#		CFLAGS=\"-I${CMAKE_INSTALL_PREFIX}/include ${OPT_FLAGS} $ENV{CFLAGS}\" 
#		CXXFLAGS=\"-I${CMAKE_INSTALL_PREFIX}/include ${OPT_FLAGS} $ENV{CXXFLAGS}\" 
#		LDFLAGS=\"-L${CMAKE_INSTALL_PREFIX}/lib $ENV{LDFLAGS}\"")
# 	BUILD_COMMAND make
 	INSTALL_COMMAND ""
 	)

#ExternalProject_Get_Property(lcm CONFIGURE_COMMAND)
#message(FATAL_ERROR ${CONFIGURE_COMMAND})

add_custom_target(install)  # define a blank install target that does nothing (just so "make install doesn't fail")
