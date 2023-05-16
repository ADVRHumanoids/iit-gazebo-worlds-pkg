cmake_minimum_required(VERSION 3.7.2)
project(iit_gazebo_worlds_pkg)

find_package(catkin REQUIRED COMPONENTS
  rospy
  gazebo_ros
)

catkin_package()

catkin_python_setup()

install(DIRECTORY launch models worlds
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
