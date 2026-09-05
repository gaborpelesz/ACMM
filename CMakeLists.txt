cmake_minimum_required (VERSION 2.8)
project (ACMM)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_CUDA_STANDARD 20)

include(CheckLanguage)
check_language(CUDA)
if (CMAKE_CUDA_COMPILER)
    enable_language(CUDA)
    message(" -- CUDA FOUND")
    set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} -O3 --use_fast_math --maxrregcount=128 --ptxas-options=-v --compiler-options -Wall")
else()
    message(FATAL_ERROR "-- CUDA NOT FOUND: Fatal error")
endif()

find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED core imgproc imgcodecs)

include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(.)

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()


# For compilation ...
# Specify target & source files to compile it from
add_executable(
    ACMM
    main.h
    ACMM.h
    ACMM.cpp
    ACMM.cu
    main.cpp
    )

# For linking ...
# Specify target & libraries to link it with
target_link_libraries(ACMM
    ${OpenCV_LIBS}
    )

target_include_directories(ACMM
    PUBLIC
    ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES}
    )
