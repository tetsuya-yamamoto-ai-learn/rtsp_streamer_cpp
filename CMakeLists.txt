cmake_minimum_required(VERSION 3.8)
project(rtsp_streamer_cpp)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

# for gstreamer
find_package(PkgConfig)
pkg_check_modules(GSTREAMER REQUIRED gstreamer-1.0 gstreamer-app-1.0)

# rtsp_streamer setting
add_executable(rtsp_streamer src/rtsp_streamer.cpp)
target_include_directories(rtsp_streamer PUBLIC
 "${GSTREAMER_INCLUDE_DIRECTORIES}")
target_link_libraries(rtsp_streamer
 "${GSTREAMER_LIBRARIES}")

# add ament target dependencies
ament_target_dependencies(rtsp_streamer rclcpp std_msgs sensor_msgs)

# add executable
install(TARGETS
  rtsp_streamer
  DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
