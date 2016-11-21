cmake_minimum_required (VERSION 2.8)
project (kmeans)
set(CMAKE_BUILD_TYPE Debug)

# The version number.
set (CUDA-CNN_VERSION_MAJOR 2)
set (CUDA-CNN_VERSION_MINOR 0)
set(DEP_LIB_PATH ${PROJECT_SOURCE_DIR}/lib)
MESSAGE("${CUDA_INCLUDE_DIRS}/../samples/common/inc")
include_directories ("${CUDA_INCLUDE_DIRS}/../samples/common/inc")

#include project 
include_directories(${PROJECT_SOURCE_DIR})
MESSAGE(${PROJECT_SOURCE_DIR})
include_directories("${PROJECT_SOURCE_DIR}/inc/")

#CUDA
find_package (CUDA REQUIRED)
include_directories (${CUDA_INCLUDE_DIRS})
include_directories ("${CUDA_INCLUDE_DIRS}/../samples/common/inc")
set(CUDA_SEPARABLE_COMPILATION ON)

#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -g -fPIC -m64 -Wall -D__TRAIN_DEBUG__")
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D__TRAIN_OUTPUT_RESULT__")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3 -fPIC -m64")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mavx -ffast-math")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

#set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} -std=c++11")
set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS}
    -gencode arch=compute_20,code=sm_20
    -gencode arch=compute_20,code=sm_21
    -gencode arch=compute_30,code=sm_30
    -gencode arch=compute_35,code=sm_35
    -gencode arch=compute_50,code=sm_50
    -gencode arch=compute_52,code=compute_52
    -rdc=true
    )

set(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS})
SET(CUDA_PROPAGATE_HOST_FLAGS OFF)
MESSAGE(${CUDA_NVCC_FLAGS})

file(GLOB CPP_SOURCES  RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} 
    "src/*.cc"
    "src/*.cu"
    "src/*.cpp"
    )

cuda_add_executable(kmeans ${CPP_SOURCES})

target_link_libraries(kmeans
    ${CUDA_LIBRARIES}
    )
