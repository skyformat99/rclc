cmake_minimum_required(VERSION 3.5)

project(rclc)

find_package(ament_cmake_ros REQUIRED)
find_package(rcl REQUIRED)
find_package(rosidl_generator_c REQUIRED)

ament_export_include_directories(include)
include_directories(include)

if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 11)
endif()

set(rclc_sources
  "src/functions.c"
)

add_library(${PROJECT_NAME}
  SHARED
  ${rclc_sources}
)

ament_target_dependencies(${PROJECT_NAME}
  rcl
  rosidl_generator_c
)

ament_export_dependencies(
  rcl
  rmw
  rosidl_generator_c
)
ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()

  add_subdirectory(test)
endif()

ament_package()

install(
  DIRECTORY include/
  DESTINATION include
)
install(
  TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)
