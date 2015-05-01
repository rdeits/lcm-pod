
add_executable(lcm-logger lcm_logger.c glib_util.c)
target_link_libraries(lcm-logger lcm)
pods_install_executables(lcm-logger)

add_executable(lcm-logplayer lcm_logplayer.c)
target_link_libraries(lcm-logplayer lcm)
pods_install_executables(lcm-logplayer)

if (WIN32)
  set_source_files_properties(lcm_logger.c glib_util.c lcm_logplayer.c PROPERTIES COMPILE_FLAGS /TP)
  target_link_libraries(lcm-logger getopt)
  target_link_libraries(lcm-logplayer getopt)
endif(WIN32)
