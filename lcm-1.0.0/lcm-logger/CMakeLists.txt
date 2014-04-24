
add_executable(lcm-logger lcm_logger.c glib_util.c)
target_link_libraries(lcm-logger lcm)
pods_use_pkg_config_packages(lcm-logger glib-2.0 gthread-2.0)

add_executable(lcm-logplayer lcm_logplayer.c)
target_link_libraries(lcm-logplayer lcm)
pods_use_pkg_config_packages(lcm-logplayer glib-2.0 gthread-2.0)

pods_install_executables(lcm-logger lcm-logplayer)

