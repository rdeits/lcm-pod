cmake_minimum_required(VERSION 2.8.6) # for UseJava.cmake

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# set up pods
set(POD_NAME lcm)
include(cmake/pods.cmake)

# set up Java, use a target of 1.6
find_package(Java REQUIRED)
include(UseJava)
set(CMAKE_JAVA_COMPILE_FLAGS ${CMAKE_JAVA_COMPILE_FLAGS} -source 6 -target 6)

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
 if (APPLE)  # this was a step towards getting things to work with
#   clang on mac, but ultimately we didn't get there...   (but I would
#   be worried about sharing pointers between objects compiled against
#   different c++ libs, so removing it)
#    # http://stackoverflow.com/questions/13445742/apple-and-shared-ptr
   set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")  
  endif()
endif()

enable_testing()
include(CTest)

