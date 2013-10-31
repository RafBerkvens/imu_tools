cmake_minimum_required(VERSION 2.8.3)
project(rviz_imu_plugin)

find_package(catkin REQUIRED COMPONENTS rviz)

## This plugin includes Qt widgets, so we must include Qt like so:
find_package(Qt4 COMPONENTS QtCore QtGui REQUIRED)
include(${QT_USE_FILE})

## I prefer the Qt signals and slots to avoid defining "emit", "slots",
## etc because they can conflict with boost signals, so define QT_NO_KEYWORDS here.
add_definitions(-DQT_NO_KEYWORDS)

## Here we specify which header files need to be run through "moc",
## Qt's meta-object compiler.
qt4_wrap_cpp(MOC_FILES
  include/rviz_imu_plugin/imu_display.h
)

## Here we specify the list of source files
set(SOURCE_FILES  src/imu_display.cpp
                  src/imu_orientation_visual.cpp
                  src/imu_axes_visual.cpp
                  src/imu_acc_visual.cpp
		  ${MOC_FILES}
)

include_directories(${rviz_imu_plugin_SOURCE_DIR}/include)

catkin_package(CATKIN_DEPENDS rviz)

## Build libraries
add_library(rviz_imu_plugin ${SOURCE_FILES})

## Link the library with whatever Qt libraries have been defined by
## the ``find_package(Qt4 ...)`` line above.
target_link_libraries(rviz_imu_plugin ${QT_LIBRARIES})

