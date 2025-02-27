# Copyright (c) OpenMMLab. All rights reserved.
cmake_minimum_required(VERSION 3.14)
project(mmdeploy_trt_net)

include(${CMAKE_SOURCE_DIR}/cmake/MMDeploy.cmake)
include(${CMAKE_SOURCE_DIR}/cmake/tensorrt.cmake)

mmdeploy_add_module(${PROJECT_NAME} trt_net.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE
        ${TENSORRT_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PRIVATE ${CUDNN_DIR}/include)
target_include_directories(${PROJECT_NAME} PRIVATE ${CUDA_TOOLKIT_ROOT_DIR}/include)
target_link_directories(${PROJECT_NAME} PUBLIC ${CUDNN_DIR}/lib64 ${CUDNN_DIR}/lib/x64)
target_link_libraries(${PROJECT_NAME} PRIVATE mmdeploy_tensorrt_ops_obj)
target_link_libraries(${PROJECT_NAME} PUBLIC ${TENSORRT_LIBRARY} cudnn)

add_library(mmdeploy::trt_net ALIAS ${PROJECT_NAME})
