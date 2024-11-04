cmake_minimum_required(VERSION 2.8.3)
project(diff_drive)

find_package(catkin REQUIRED COMPONENTS
  rospy
  std_msgs
  nav_msgs
  geometry_msgs
  tf
  tf2
  actionlib
  actionlib_msgs
  dynamic_reconfigure
)

add_action_files(
  DIRECTORY action
  FILES GoToPose.action
)

if (CATKIN_ENABLE_TESTING)
  catkin_add_nosetests(tests)
endif()

if (CATKIN_ENABLE_TESTING)
  find_package(rostest REQUIRED)
#  add_rostest(tests/your_first_rostest.test)
#  add_rostest(tests/your_second_rostest.test)
endif()

catkin_install_python(
    PROGRAMS
        #nodes/diff_drive_odometry
        #nodes/diff_drive_controller
        nodes/diff_drive_go_to_goal
	      #nodes/diff_drive_mock_robot
    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

catkin_python_setup()

generate_messages (
  DEPENDENCIES actionlib_msgs std_msgs geometry_msgs
)

################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a exec_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed

# Generate dynamic reconfigure parameters in the 'cfg' folder
# Must be called before catkin_package()
generate_dynamic_reconfigure_options(
  cfg/pid.cfg 
)

catkin_package(
  CATKIN_DEPENDS actionlib_msgs dynamic_reconfigure
)
