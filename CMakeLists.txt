cmake_minimum_required(VERSION 2.8.3)
project(block_grasp_generator)

find_package(Eigen REQUIRED)
find_package(Boost REQUIRED thread system)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED
  roscpp
  std_msgs
  sensor_msgs
  visualization_msgs 
  eigen_conversions 
  moveit_ros_planning 
  moveit_ros_planning_interface
  moveit_ros_robot_interaction
  moveit_core
  tf
  tf_conversions
)

# Grasp Generator Library
add_library(${PROJECT_NAME}_block_grasp_generator
  src/block_grasp_generator.cpp
  src/grasp_filter.cpp
)
target_link_libraries(${PROJECT_NAME}_block_grasp_generator ${catkin_LIBRARIES} ${Boost_LIBRARIES})

# Grasp Filter Library
add_library(${PROJECT_NAME}_grasp_filter
  src/grasp_filter.cpp
)
target_link_libraries(${PROJECT_NAME}_grasp_filter ${catkin_LIBRARIES} ${Boost_LIBRARIES})

# Catkin
catkin_package(
  LIBRARIES
    ${PROJECT_NAME}_block_grasp_generator
    ${PROJECT_NAME}_grasp_filter
  CATKIN_DEPENDS
    roscpp
    std_msgs
    sensor_msgs
    visualization_msgs 
    eigen_conversions 
    moveit_ros_planning 
    moveit_ros_planning_interface
    moveit_ros_robot_interaction
    moveit_core
    tf
    tf_conversions
  INCLUDE_DIRS include
)

# Enable gdb to show line numbers
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -g")
#set(CMAKE_BUILD_TYPE Debug)

## Build 
include_directories(
  include 
  ${catkin_INCLUDE_DIRS}
)
link_directories(${catkin_LIBRARY_DIRS})

# Test executable
add_executable(block_grasp_generator_test src/block_grasp_generator_test.cpp)
target_link_libraries(block_grasp_generator_test ${catkin_LIBRARIES} ${PROJECT_NAME}_block_grasp_generator)

# Test executable
add_executable(grasp_filter_test src/grasp_filter_test.cpp)
target_link_libraries(grasp_filter_test ${catkin_LIBRARIES} ${PROJECT_NAME}_block_grasp_generator ${PROJECT_NAME}_grasp_filter)

# Test executable
add_executable(robot_viz_tool_test src/robot_viz_tool_test.cpp)
target_link_libraries(robot_viz_tool_test ${catkin_LIBRARIES} ${PROJECT_NAME}_block_grasp_generator ${PROJECT_NAME}_grasp_filter)


