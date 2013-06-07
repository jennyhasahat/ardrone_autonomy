cmake_minimum_required(VERSION 2.8.3)
project(ardrone_autonomy)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS roscpp sensor_msgs std_srvs camera_info_manager image_transport tf message_generation genmsg)

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED COMPONENTS system)
#find_package(libsdl-dev REQUIRED COMPONENTS system)

## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/groovy/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

#######################################
## Declare ROS messages and services ##
#######################################

# Generate messages in the 'msg' folder
  add_message_files(
	FILES
	matrix33.msg
	navdata_references.msg
	navdata_adc_data_frame.msg
	navdata_time.msg
	navdata_altitude.msg
	navdata_trackers_send.msg
	navdata_demo.msg
	navdata_trims.msg
	navdata_euler_angles.msg
	navdata_video_stream.msg
	navdata_games.msg
	navdata_vision_detect.msg
	navdata_gyros_offsets.msg
	navdata_vision.msg
	navdata_hdvideo_stream.msg
	navdata_vision_of.msg
	navdata_kalman_pressure.msg
	navdata_vision_perf.msg
	navdata_magneto.msg
	navdata_vision_raw.msg
	Navdata.msg
	navdata_watchdog.msg
	navdata_phys_measures.msg
	navdata_wifi.msg
	navdata_pressure_raw.msg
	navdata_wind_speed.msg
	navdata_pwm.msg
	navdata_zimmu_3000.msg
	navdata_raw_measures.msg
	vector21.msg
	navdata_rc_references.msg
	vector31.msg
)

# Generate services in the 'srv' folder
add_service_files(
	FILES
	CamSelect.srv
	FlightAnim.srv
	LedAnim.srv
)

# Generate added messages and services with any dependencies listed here
 generate_messages(
   DEPENDENCIES
   sensor_msgs
 )


###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#   INCLUDE_DIRS include
#  LIBRARIES ardrone_autonomy
  CATKIN_DEPENDS camera_info_manager image_transport roscpp sensor_msgs std_srvs tf message_runtime
  DEPENDS libsdl-dev pthread
)

###########
## Build ##
###########

set(SDK ARDroneLib/)

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(include
  ${catkin_INCLUDE_DIRS}
  ${SDK} ${SDK}/FFMPEG/Includes ${SDK}/Soft/Common ${SDK}/Soft/Lib ${SDK}/VP_SDK ${SDK}/VP_SDK/VP_Os/linux
)
link_directories(${PROJECT_SOURCE_DIR}/lib/)

## Declare a cpp library
# add_library(ardrone_autonomy
#   src/${PROJECT_NAME}/ardrone_autonomy.cpp
# )

## Declare a cpp executable
# add_executable(ardrone_autonomy_node src/ardrone_autonomy_node.cpp)
add_executable(ardrone_driver src/ardrone_driver.cpp src/video.cpp src/ardrone_sdk.cpp src/teleop_twist.cpp)

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(ardrone_autonomy_node ardrone_autonomy_generate_messages_cpp)

## Specify libraries to link a library or executable target against
 target_link_libraries(ardrone_driver
   ${catkin_LIBRARIES}
 )
target_link_libraries(ardrone_driver pc_ardrone avcodec avutil swscale vlib sdk SDL pthread)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/groovy/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS ardrone_autonomy ardrone_autonomy_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_ardrone_autonomy.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
