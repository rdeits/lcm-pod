add_custom_target(install_python ALL
	COMMAND python setup.py build --build-base=${CMAKE_CURRENT_BINARY_DIR} install --prefix=${CMAKE_INSTALL_PREFIX} 
	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})