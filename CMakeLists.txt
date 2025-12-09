cmake_minimum_required(VERSION 3.10)
project(APD)

set(Boost_USE_STATIC_LIBS ON)
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME OFF)

find_package(OpenCV REQUIRED)
find_package(Boost REQUIRED COMPONENTS filesystem system)
find_package(CUDA REQUIRED)

include_directories(
    ${OpenCV_INCLUDE_DIRS}
    ${Boost_INCLUDE_DIRS}
)
include_directories(.)

set(CUDA_NVCC_FLAGS
    ${CUDA_NVCC_FLAGS};
    -O3 --use_fast_math --maxrregcount=128 --ptxas-options=-v -std=c++11
    --compiler-options -Wall
)

if(CMAKE_COMPILER_IS_GNUCXX)
    add_definitions(-std=c++11 -pthread -Wall -Wextra -pedantic)
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3 -ffast-math -march=native")
endif()

cuda_add_executable(
    APD
    main.cpp
    main.h
    APD.cpp
    APD.cu
    APD.h
)

target_link_libraries(
    APD
    ${Boost_LIBRARIES}
    ${OpenCV_LIBS}
)
