cmake_minimum_required(VERSION 3.0)
project(aloam_velodyne)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++14")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  tf
  std_msgs
  geometry_msgs
  nav_msgs
  sensor_msgs
  pcl_ros
)

find_package(Eigen3 REQUIRED QUIET)
find_package(PCL REQUIRED QUIET)
find_package(Ceres REQUIRED QUIET)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS 
    roscpp
    tf
    std_msgs
    geometry_msgs
    nav_msgs
    sensor_msgs
    pcl_ros
  DEPENDS 
    EIGEN3 
    PCL
    CERES
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  ${PCL_INCLUDE_DIRS}
)

add_executable(ascanRegistration 
  src/scanRegistration.cpp
)
target_link_libraries(ascanRegistration 
  ${catkin_LIBRARIES} 
  ${PCL_LIBRARIES}
)

add_executable(alaserOdometry 
  src/laserOdometry.cpp
)
target_link_libraries(alaserOdometry 
  ${catkin_LIBRARIES} 
  ${PCL_LIBRARIES} 
  Ceres::ceres
)

add_executable(alaserMapping 
  src/laserMapping.cpp
)
target_link_libraries(alaserMapping 
  ${catkin_LIBRARIES} 
  ${PCL_LIBRARIES} 
  Ceres::ceres
)
