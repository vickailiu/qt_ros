##############################################################################
# CMake
##############################################################################

cmake_minimum_required(VERSION 3.1)
project(qt_tutorials)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
find_package(catkin)

##############################################################################
# Catkin Configuration
##############################################################################

# These find_package calls setup catkin_INCLUDE_DIRS and ROS_LIBRARIES and 
# all the component variables too (e.g. roscpp_INCLUDE_DIR?)
find_package(catkin REQUIRED COMPONENTS message_generation std_msgs roscpp)
include_directories(${catkin_INCLUDE_DIRS} ${CATKIN_GLOBAL_INCLUDE_DESTINATION})

##############################################################################
# Qt Environment
##############################################################################

find_package(Qt5Widgets REQUIRED)
find_package(Qt5Core REQUIRED)
include_directories(${Qt5Widgets_INCLUDE_DIRS})

##############################################################################
# Services
##############################################################################

add_service_files(DIRECTORY srv FILES TwoInts.srv)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(CATKIN_DEPENDS message_runtime std_msgs roscpp)

##############################################################################
# Subprojects
##############################################################################

foreach(subdir
    common
    qtalker
    qlistener
    qadd_server
    qadd_client
    )
  #install(DIRECTORY ${subdir} DESTINATION ${CMAKE_INSTALL_PREFIX}/share/qt_tutorials)
  add_subdirectory(${subdir})
endforeach()

##############################################################################
# Optional Installs - these aren't really necessary
##############################################################################

install(DIRECTORY resources DESTINATION ${CMAKE_INSTALL_PREFIX}/share/qt_tutorials)
install(DIRECTORY ui DESTINATION ${CMAKE_INSTALL_PREFIX}/share/qt_tutorials)

