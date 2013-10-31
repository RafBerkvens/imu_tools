cmake_minimum_required(VERSION 2.8.3)
project(imu_filter_madgwick)

find_package(catkin REQUIRED COMPONENTS roscpp sensor_msgs geometry_msgs tf
    nodelet message_filters dynamic_reconfigure)

# Generate dynamic parameters
generate_dynamic_reconfigure_options(cfg/ImuFilterMadgwick.cfg)

# Declare that this catkin package's runtime dependencies
catkin_package(CATKIN_DEPENDS roscpp sensor_msgs geometry_msgs tf nodelet 
    message_filters dynamic_reconfigure)

include_directories(${Boost_INCLUDE_DIRS})
include_directories(${imu_filter_madgwick_SOURCE_DIR}/include)

# create imu_filter library
add_library(imu_filter src/imu_filter.cpp)
target_link_libraries(imu_filter ${Boost_LIBRARIES})

# create imu_filter_nodelet library
add_library(imu_filter_nodelet src/imu_filter_nodelet.cpp)
target_link_libraries(imu_filter_nodelet imu_filter)

# create imu_filter_node executable
add_executable(imu_filter_node src/imu_filter_node.cpp)
target_link_libraries(imu_filter_node imu_filter ${catkin_LIBRARIES})