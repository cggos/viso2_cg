cmake_minimum_required(VERSION 2.8.3)
project(viso2)

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")

if(CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64")
    add_definitions(-msse3)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "arm")
    SET(CMAKE_CXX_FLAGS -mfpu=neon)
endif()

include_directories(include)

add_library(viso2 SHARED
        src/filter.cpp
        src/matcher.cpp
        src/matrix.cpp
        src/reconstruction.cpp
        src/triangle.cpp
        src/viso.cpp
        src/viso_mono.cpp
        src/viso_stereo.cpp)

if(BUILD_DEMO)
    add_executable(viso2_demo src/demo.cpp)
    target_link_libraries (viso2_demo viso2 png)
endif()

