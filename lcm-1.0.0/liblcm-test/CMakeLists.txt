
foreach(test-name IN ITEMS lcm-sink lcm-source lcm-tester lcm-example lcm-logfilter buftest-receiver buftest-sender)
  add_executable(${test-name} ${test-name}.c)
  target_link_libraries(${test-name} lcm)
  pods_use_pkg_config_packages(${test-name} glib-2.0)
  add_test(NAME ${test-name} COMMAND ${test-name})
endforeach()
