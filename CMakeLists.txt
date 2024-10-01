cmake_minimum_required(VERSION 3.12)
set(PROJECT_NAME yolov11-trt)
project(PROJECT_NAME)

find_package(CUDA REQUIRED)

set(CMAKE_CXX_STANDARD 17)
add_definitions(-DAPI_EXPORTS)

include_directories(${CUDA_INCLUDE_DIRS})
include_directories(src/)

# Add source files
set(SOURCES
    main.cpp
    src/yolov11.cpp
)

# Add CUDA source files
set(CUDA_SOURCES src/preprocess.cu)

# Add headers
set(HEADERS
    src/yolov11.h
    src/macros.h
    src/logging.h
    src/cuda_utils.h
    src/preprocess.h
    src/common.h
)

# Find and include OpenCV
if(NOT DEFINED ENV{OpenCV_DIR})
    message(FATAL_ERROR "Environment variable OpenCV_DIR is not set. Please set it to your OpenCV build directory path.")
else()
    set(OpenCV_DIR $ENV{OpenCV_DIR})
endif()

# Include OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# Find and include TensorRT
if(NOT DEFINED ENV{TENSORRT_DIR})
    message(FATAL_ERROR "Environment variable TENSORRT_DIR is not set. Please set it to your TensorRT directory path.")
else()
    set(TENSORRT_DIR $ENV{TENSORRT_DIR})
endif()

# Include TensorRT
include_directories(${TENSORRT_DIR}/include)
link_directories(${TENSORRT_DIR}/lib)
set(TENSORRT_LIBS nvinfer nvinfer_plugin nvparsers nvonnxparser)

# Create an executable
cuda_add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS} ${CUDA_SOURCES})

# Link libraries
target_link_libraries(${PROJECT_NAME}
    ${OpenCV_LIBS}
    ${CUDA_LIBRARIES}
    ${TENSORRT_LIBS}
)