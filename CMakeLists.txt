cmake_minimum_required(VERSION 3.0.2)
project(rt_obstacle_layer)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  cmake_modules
  costmap_2d
  dynamic_reconfigure
  geometry_msgs
  laser_geometry
  map_msgs
  message_filters
  message_generation
  nav_msgs
  pluginlib
  roscpp
  sensor_msgs
  std_msgs
  tf2
  tf2_geometry_msgs
  tf2_ros
  tf2_sensor_msgs
  visualization_msgs
  voxel_grid
)
find_package(Boost REQUIRED COMPONENTS system thread)

## Generate dynamic reconfigure parameters in the 'cfg' folder
generate_dynamic_reconfigure_options(
  cfg/RtObstacleLayer.cfg
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS 
    include
  LIBRARIES 
    rt_obstacle_layer
  CATKIN_DEPENDS 
    cmake_modules 
    costmap_2d 
    dynamic_reconfigure 
    geometry_msgs 
    laser_geometry 
    map_msgs 
    message_filters 
    nav_msgs 
    pluginlib 
    roscpp 
    sensor_msgs 
    std_msgs 
    tf2 
    tf2_geometry_msgs 
    tf2_ros 
    tf2_sensor_msgs 
    visualization_msgs 
    voxel_grid
  DEPENDS 
    system_lib
    Boost
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

## Declare a C++ library
add_library(
  ${PROJECT_NAME}
  src/rt_obstacle_layer.cpp
  src/rt_observation_buffer.cpp
)

target_link_libraries(
  rt_obstacle_layer 
  ${catkin_LIBRARIES}
)

add_dependencies(
  rt_obstacle_layer 
  ${rt_obstacle_layer_EXPORTED_TARGETS}
  ${catkin_EXPORTED_TARGETS}
)

install(TARGETS rt_obstacle_layer
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(FILES rt_obstacle_layer_plugins.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(DIRECTORY cfg
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)