
 
add_library(lcm
	lcm.c 
	lcm_udp.c 
	lcm_file.c 
	lcm_tcpq.c 
	ringbuffer.c
	eventlog.c )

set(REQUIRED_LIBS glib-2.0 gthread-2.0)
pods_use_pkg_config_packages(lcm ${REQUIRED_LIBS})

pods_install_libraries(lcm)

pods_install_headers(eventlog.h 
				lcm.h 
				lcm_coretypes.h
				lcm-cpp.hpp 
				lcm-cpp-impl.hpp 
				DESTINATION lcm)

pods_install_pkg_config_file(lcm
    LIBS -llcm
    REQUIRES ${REQUIRED_LIBS}
    VERSION ${LCM_VERSION})
