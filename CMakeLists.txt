cmake_minimum_required(VERSION 2.8.3)
project(viso2_cg)

add_subdirectory(viso2)

find_package(catkin REQUIRED COMPONENTS
    roscpp
    sensor_msgs
    nav_msgs
    message_filters
    image_transport
    cv_bridge
    image_geometry
    tf
    pcl_ros
    std_srvs
    message_generation
)

find_package(Boost REQUIRED COMPONENTS signals thread)
include_directories(${Boost_INCLUDE_DIRS})

add_message_files(DIRECTORY msg FILES VisoInfo.msg)

generate_messages(DEPENDENCIES std_msgs)

catkin_package()

include_directories(include viso2/include ${catkin_INCLUDE_DIRS})

add_executable(stereo_odometer src/stereo_odometer.cpp)
add_dependencies(stereo_odometer ${catkin_EXPORTED_TARGETS} ${PROJECT_NAME}_gencpp)
target_link_libraries(stereo_odometer viso2 ${Boost_LIBRARIES} ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})

add_executable(mono_odometer src/mono_odometer.cpp)
add_dependencies(mono_odometer ${catkin_EXPORTED_TARGETS} ${PROJECT_NAME}_gencpp)
target_link_libraries(mono_odometer viso2 ${OpenCV_LIBRARIES} ${catkin_LIBRARIES})
