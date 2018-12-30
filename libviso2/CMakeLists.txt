cmake_minimum_required(VERSION 2.8.3)
project(libviso2)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")

set(USE_ROS TRUE)

if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64")
    add_definitions(-msse3)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "arm")
    SET(CMAKE_CXX_FLAGS -mfpu=neon)
endif()

if(USE_ROS)
    find_package(catkin REQUIRED )
    catkin_package(
            INCLUDE_DIRS include
            LIBRARIES viso2)
    include_directories(${catkin_INCLUDE_DIRS})
endif()

include_directories(include)

add_library(viso2
        src/filter.cpp
        src/matcher.cpp
        src/matrix.cpp
        src/reconstruction.cpp
        src/triangle.cpp
        src/viso.cpp
        src/viso_mono.cpp
        src/viso_stereo.cpp)

add_executable(viso2_demo src/demo.cpp)
target_link_libraries (viso2_demo viso2 png)

if(USE_ROS)
    install(TARGETS viso2
            ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
            LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
            RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
    install(DIRECTORY include
            DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
            PATTERN ".cpp" EXCLUDE)
endif()

