cmake_minimum_required(VERSION 3.10.2)
project(iit_gazebo_worlds_pkg)

find_package(ament_cmake_python REQUIRED)
find_package(rclpy REQUIRED)
find_package(ros_gz_sim REQUIRED)

# install(PROGRAMS
#   scripts/pose_gazebo_to_tf
#   DESTINATION lib/${PROJECT_NAME}
# )

install(
  DIRECTORY 3DGems genericShapes table
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
