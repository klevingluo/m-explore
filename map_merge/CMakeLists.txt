cmake_minimum_required(VERSION 2.8.3)
project(multirobot_map_merge)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
  geometry_msgs
  nav_msgs
  map_msgs
  tf2_geometry_msgs
)

find_package(Boost REQUIRED COMPONENTS thread)

# OpenCV is required for merging without initial positions
set(CMAKE_BUILD_TYPE Debug)
find_package(OpenCV 3 REQUIRED)
if(OpenCV_VERSION VERSION_LESS "3.0")
  message(FATAL_ERROR "This package needs OpenCV >= 3.0")
endif()

################################################
## Declare ROS messages, services and actions ##
################################################
# we don't have any

###################################
## catkin specific configuration ##
###################################
catkin_package()

###########
## Build ##
###########
# c++11 support required
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
if(COMPILER_SUPPORTS_CXX11)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
  message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

find_package(OpenCV 3 REQUIRED)
if(OpenCV_VERSION VERSION_LESS "3.0")
  message(FATAL_ERROR "This package needs OpenCV >= 3.0")
endif()

## Specify additional locations of header files
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  include
)
message("${OpenCV_VERSION}")
message("${OpenCV_VERSION}")
message("${OpenCV_VERSION}")
message("${OpenCV_VERSION}")
message("${OpenCV_VERSION}")
message("${OpenCV_VERSION}")

FIND_FILE(MYLIB libopencv_stitching.so /usr/local/lib)

add_executable(map_merge
  src/map_merge.cpp
  src/combine_grids/merging_pipeline.cpp
  src/combine_grids/grid_warper.cpp
  src/combine_grids/grid_compositor.cpp
)

target_link_libraries(map_merge 
  ${OpenCV_LIBS}
  ${catkin_LIBRARIES}
  "~/robotics/src/m-explore/map_merge/libopencv_stitching.so"
)

#############
## Install ##
#############

# install roslaunch files
install(DIRECTORY launch/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)
