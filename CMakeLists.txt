cmake_minimum_required(VERSION 3.5)
project(robot_localization)

if(NOT WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  message(STATUS "${PROJECT_NAME}: You did not request a specific build type: selecting 'RelWithDebInfo'.")
  set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif()

find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(builtin_interfaces REQUIRED)

find_package(geometry_msgs REQUIRED)
find_package(geographic_msgs REQUIRED)
find_package(diagnostic_msgs REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "srv/FromLL.srv"
  "srv/FromLLArr.srv"
  "srv/GetState.srv"
  "srv/SetDatum.srv"
  "srv/SetPose.srv"
  "srv/ToggleFilterProcessing.srv"
  "srv/ToLL.srv"
  "srv/ToLLArr.srv"
  DEPENDENCIES
    builtin_interfaces
    geometry_msgs
    geographic_msgs
    diagnostic_msgs
  ADD_LINTER_TESTS
)

if(BUILD_TESTING)
  # find_package(ament_cmake_cppcheck REQUIRED)
  # find_package(ament_cmake_cpplint REQUIRED)
  find_package(ament_cmake_lint_cmake REQUIRED)
  # find_package(ament_cmake_uncrustify REQUIRED)

  # ament_cppcheck(LANGUAGE "c++")
  # ament_cpplint()
  ament_lint_cmake()
  # ament_uncrustify()

endif()

ament_export_dependencies(rosidl_default_runtime)
ament_package()
