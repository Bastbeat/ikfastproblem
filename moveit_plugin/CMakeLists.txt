cmake_minimum_required(VERSION 2.8.3)
project(moveit_plugin)

find_package(Eigen REQUIRED)
include_directories(SYSTEM ${EIGEN_INCLUDE_DIRS})

find_package(catkin REQUIRED COMPONENTS
  moveit_core
  pluginlib
  roscpp
  tf_conversions
)

include_directories(${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

catkin_package(
  LIBRARIES
  CATKIN_DEPENDS
    moveit_core
    pluginlib
    roscpp
    tf_conversions
)

include_directories(include)

set(IKFAST_LIBRARY_NAME motoman_mh5_planning_group_moveit_ikfast_plugin)

find_package(LAPACK REQUIRED)

add_library(${IKFAST_LIBRARY_NAME} src/motoman_mh5_planning_group_ikfast_moveit_plugin.cpp)
target_link_libraries(${IKFAST_LIBRARY_NAME} ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${LAPACK_LIBRARIES})

install(TARGETS ${IKFAST_LIBRARY_NAME} LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(
  FILES
  motoman_mh5_moveit_ikfast_plugin_description.xml
  DESTINATION
  ${CATKIN_PACKAGE_SHARE_DESTINATION}
)