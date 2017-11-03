cmake_minimum_required(VERSION 2.8.3)
project(ira_laser_tools)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  cmake_modules
  dynamic_reconfigure
  laser_geometry
  pcl_conversions
  pcl_ros
  sensor_msgs
  std_msgs
  tf
)

# Fallback to cmake_modules
find_package(cmake_modules REQUIRED)
find_package(Eigen REQUIRED)

#find_package(PCL REQUIRED)

generate_dynamic_reconfigure_options(cfg/laserscan_multi_merger.cfg cfg/laserscan_virtualizer.cfg)

###################################
## catkin specific configuration ##
###################################
catkin_package(
  INCLUDE_DIRS
#  LIBRARIES laser_merger
  CATKIN_DEPENDS 
    roscpp
    cmake_modules
    dynamic_reconfigure
    laser_geometry
    pcl_conversions
    pcl_ros
    sensor_msgs
    std_msgs
    tf
  DEPENDS Eigen
)

###########
## Build ##
###########
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
#  ${PCL_INCLUDE_DIRS}
)

add_executable(laserscan_multi_merger src/laserscan_multi_merger.cpp)
add_dependencies(laserscan_multi_merger ${PROJECT_NAME}_gencfg)
target_link_libraries(laserscan_multi_merger
  ${catkin_LIBRARIES}
#  ${PCL_LIBRARIES}
)

add_executable(laserscan_virtualizer src/laserscan_virtualizer.cpp)
add_dependencies(laserscan_virtualizer ${PROJECT_NAME}_gencfg)
target_link_libraries(laserscan_virtualizer
  ${catkin_LIBRARIES}
#  ${PCL_LIBRARIES}
)

#############
## Install ##
#############
## Mark executables and/or libraries for installation
install(TARGETS laserscan_multi_merger laserscan_virtualizer
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

## Mark other files for installation (e.g. launch and bag files, etc.)
install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
)
