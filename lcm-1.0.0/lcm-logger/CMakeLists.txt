
if (NOT WIN32) # have to resolve the g_thread_init linking error to get this out
   add_executable(lcm-logger lcm_logger.c glib_util.c)
   target_link_libraries(lcm-logger lcm getopt)
   pods_use_pkg_config_packages(lcm-logger glib-2.0 gthread-2.0)
   pods_install_executables(lcm-logger)
endif()

add_executable(lcm-logplayer lcm_logplayer.c)
target_link_libraries(lcm-logplayer lcm getopt)
pods_use_pkg_config_packages(lcm-logplayer glib-2.0 gthread-2.0)
pods_install_executables(lcm-logplayer)

if (WIN32)
   set_source_files_properties(lcm_logger.c glib_util.c lcm_logplayer.c PROPERTIES COMPILE_FLAGS /TP)
endif()


