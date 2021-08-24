cmake_minimum_required(VERSION 3.0.2)
project(d435_heightmap)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages

set(${PROJECT_NAME}_CATKIN_DEPS
    nodelet
    roscpp
    pcl_ros
    sensor_msgs
    tf)

find_package(catkin REQUIRED COMPONENTS
    nodelet
    pcl_ros
    roscpp
    sensor_msgs
    tf
    angles
    pcl_conversions
    roslaunch
    rostest
)

find_package(Boost)
find_package(PCL REQUIRED QUIET COMPONENTS common)

include_directories(include
                    ${Boost_INCLUDE_DIR}
                    ${catkin_INCLUDE_DIRS}
                    ${PCL_INCLUDE_DIR})

catkin_package(CATKIN_DEPENDS ${${PROJECT_NAME}_CATKIN_DEPS})

add_executable(heightmap_node src/heightmap_node.cpp src/heightmap.cpp)
target_link_libraries(heightmap_node
                      ${catkin_LIBRARIES}
                      ${Boost_LIBRARIES}
                      ${PCL_LIBRARIES})

add_library(heightmap_nodelet src/heightmap_nodelet.cpp src/heightmap.cpp)
target_link_libraries(heightmap_nodelet
                      ${catkin_LIBRARIES}
                      ${Boost_LIBRARIES}
                      ${PCL_LIBRARIES})

# install the files needed to run this package
install(TARGETS heightmap_node
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
install(TARGETS heightmap_nodelet
        RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})
install(FILES nodelets.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)


catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES d435_heightmap
#  CATKIN_DEPENDS nodelet pcl_ros roscpp sensor_msgs tf
#  DEPENDS system_lib
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
)