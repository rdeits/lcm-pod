cmake_minimum_required(VERSION 2.8.6) # for UseJava.cmake

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# set up pods
set(POD_NAME lcm)
set(LCM_VERSION 1.0.0)
include(cmake/pods.cmake)

if (CMAKE_COMPILER_IS_GNUCC)
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wreturn-type -Wuninitialized -Wunused-variable") # -Wunused-parameter")

   execute_process(COMMAND ${CMAKE_C_COMPILER} -dumpversion
                OUTPUT_VARIABLE GCC_VERSION)
   if (NOT (GCC_VERSION VERSION_GREATER 4.3 OR GCC_VERSION VERSION_EQUAL 4.3))
        message(FATAL_ERROR "requires gcc version >= 4.3")  # to support the c++0x flag below
   elseif(GCC_VERSION VERSION_LESS 4.7)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
   else()
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
   endif()
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wreturn-type -Wuninitialized -Wunused-variable") # -Wunused-parameter")

  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
  if (APPLE)  
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")  
  endif()
endif()

if (WIN32)
#  add_definitions(/TP)  # compile .c as .cpp for MSVC
  add_subdirectory(WinSpecific)
  include_directories(${CMAKE_CURRENT_SOURCE_DIR}/WinSpecific/include)
  include_directories(${CMAKE_CURRENT_SOURCE_DIR}/WinSpecific/getopt)
endif()


# glib and gthread
set(GLIB_REQUIRED_LIBS glib-2.0 gthread-2.0)
if (WIN32)
  if (NOT DEFINED ENV{GTK_PATH})
    message(FATAL_ERROR "ERROR: you must set the GTK_PATH environment variable")
  endif()
  include_directories($ENV{GTK_PATH}/include/glib-2.0)
  include_directories($ENV{GTK_PATH}/lib/glib-2.0/include)

  set(GLIB_LDFLAGS -LIBPATH:$ENV{GTK_PATH}/lib gthread-2.0.lib glib-2.0.lib intl.lib ws2_32.lib ole32.lib winmm.lib shlwapi.lib)
  install(FILES $ENV{GTK_PATH}/bin/libglib-2.0-0.dll RENAME glib-2.0.dll DESTINATION lib)
  install(FILES $ENV{GTK_PATH}/bin/libgthread-2.0-0.dll RENAME gthread-2.0.dll DESTINATION lib)
else()
  find_package(PkgConfig REQUIRED)
  pkg_check_modules(GLIB glib-2.0 gthread-2.0)
  if (NOT GLIB_FOUND)
     message(FATAL_ERROR "ERROR: could not find glib-2.0 and/or gthread-2.0")
  endif()
  include_directories(${GLIB_INCLUDE_DIRS})
endif()
		


enable_testing()
include(CTest)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

#add_subdirectory(m4)
add_subdirectory(lcm)
add_subdirectory(liblcm-test)
add_subdirectory(lcmgen)
add_subdirectory(lcm-logger)
#add_subdirectory(po)
#add_subdirectory(m4macros)

# set up Java, use a target of 1.6
find_package(Java)
if (JAVA_FOUND)
   include(UseJava)
   set(CMAKE_JAVA_COMPILE_FLAGS ${CMAKE_JAVA_COMPILE_FLAGS} -source 1.5 -target 1.5)

  add_subdirectory(lcm-java)
endif()

# todo: add "if python" logic
#add_subdirectory(lcm-python)

# todo: add "if lua" logic
#add_subdirectory(lcm-lua)



