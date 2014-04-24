
add_jar(lcm-java
    lcm/lcm/LCMEncodable.java 
    lcm/lcm/LCM.java 
    lcm/lcm/LCMSubscriber.java 
    lcm/lcm/URLParser.java 
    lcm/lcm/Provider.java 
    lcm/lcm/TCPProvider.java 
    lcm/lcm/UDPMulticastProvider.java 
    lcm/lcm/TCPService.java 
    lcm/lcm/LogFileProvider.java 
    lcm/lcm/TCPProvider.java 
    lcm/lcm/MessageAggregator.java 
    lcm/lcm/LCMDataInputStream.java 
    lcm/lcm/LCMDataOutputStream.java 
    lcm/util/BufferedRandomAccessFile.java 
    lcm/util/ColorMapper.java 
    lcm/util/TableSorter.java 
    lcm/util/ParameterListener.java 
    lcm/util/ParameterGUI.java 
    lcm/util/JImage.java 
    lcm/util/ClassDiscoverer.java 
    lcm/logging/JScrubber.java 
    lcm/logging/JScrubberListener.java 
    lcm/logging/Log.java 
    lcm/logging/LogDiagnostic.java 
    lcm/logging/LogPlayer.java 
    lcm/spy/Spy.java 
    lcm/spy/SpyPlugin.java 
    lcm/spy/LCMTypeDatabase.java 
    lcm/spy/ObjectPanel.java 
    lcm/spy/ChannelData.java )

pods_install_jars(lcm-java)

pods_install_pkg_config_file(lcm-java
    CLASSPATH lcm
    VERSION ${LCM_VERSION})

file(WRITE ${CMAKE_BINARY_DIR}/lcm-logplayer-gui
	"#!/bin/sh\n"
	"exec java -server -Xincgc -Xmx64m -Xms32m -ea -cp ${CMAKE_INSTALL_PREFIX}/share/java/lcm.jar lcm.logging.LogPlayer \$$*\n")
file(INSTALL ${CMAKE_BINARY_DIR}/lcm-logplayer-gui 
	     DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
	     FILE_PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)

file(WRITE ${CMAKE_BINARY_DIR}/lcm-spy
	"#!/bin/sh\n"
	"exec java -server -Xincgc -Xmx128m -Xms64m -ea -cp ${CMAKE_INSTALL_PREFIX}/share/java/lcm.jar lcm.spy.Spy \$$*\n")
file(INSTALL ${CMAKE_BINARY_DIR}/lcm-spy
	     DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
	     FILE_PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)

