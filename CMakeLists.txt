cmake_minimum_required(VERSION 3.8)
project(ros2_cv)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
find_package(std_msgs REQUIRED)

# Include Cpp "include" directory
include_directories(include)

# Create Cpp executable for each node
add_executable(computer_vision_pipeline_node src/computer_vision_pipeline_node.cpp)
ament_target_dependencies(computer_vision_pipeline_node rclcpp std_msgs)

# Install Cpp executables
install(TARGETS
  computer_vision_pipeline_node
  DESTINATION lib/${PROJECT_NAME}
)

# Install Python modules
ament_python_install_package(${PROJECT_NAME})

# Install Python executables
install(PROGRAMS
  DESTINATION lib/${PROJECT_NAME}
)

# Install data and models folders
install(DIRECTORY
  DESTINATION share/${PROJECT_NAME}
)

# Install launch files
install(FILES
  launch.xml
  DESTINATION share/${PROJECT_NAME}/
)

ament_export_dependencies(ament_cmake)
ament_export_dependencies(ament_cmake_python)
ament_export_dependencies(rclcpp)
ament_export_dependencies(rclpy)
ament_export_dependencies(std_msgs)

ament_package()