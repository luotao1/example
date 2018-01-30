project(example)
set(PADDLE_ROOT $ENV{PADDLE_ROOT} CACHE PATH "PaddlePaddle Root")
set(LIB_TYPE $ENV{LIB_TYPE} CACHE STRING "Choose the library type: static shared")
cmake_minimum_required(VERSION 2.8)
add_executable(${PROJECT_NAME} example.cc)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)
include_directories(${PADDLE_ROOT}/third_party/install/gflags/include)
include_directories(${PADDLE_ROOT}/third_party/install/glog/include)
include_directories(${PADDLE_ROOT}/third_party/install/protobuf/include)
include_directories(${PADDLE_ROOT}/third_party/eigen3)
include_directories(${PADDLE_ROOT}/)
if(LIB_TYPE STREQUAL "shared")
  target_link_libraries(${PROJECT_NAME} 
    ${PADDLE_ROOT}/paddle/inference/libpaddle_fluid.so
    -lrt -ldl -lpthread)
else()
  set(CUDA_LIB $ENV{CUDA_LIB} CACHE PATH "CUDA Library Path")
  set(CUDNN_LIB $ENV{CUDNN_LIB} CACHE PATH "CUDNN Library Path")
  target_link_libraries(${PROJECT_NAME} 
    -Wl,--whole-archive
    ${PADDLE_ROOT}/lib/libpaddle_fluid.a
    -Wl,--no-whole-archive
    ${PADDLE_ROOT}/third_party/glog/lib/libglog.a
    ${PADDLE_ROOT}/third_party/gflags/lib/libgflags.a
    ${PADDLE_ROOT}/third_party/protobuf/lib/libprotobuf-lite.a
    ${PADDLE_ROOT}/third_party/openblas/lib/libopenblas.a
    ${CUDA_LIB}/libcublas.so
    ${CUDA_LIB}/libcudart.so
    ${CUDNN_LIB}/libcudnn.so
    -lrt -ldl -lpthread)
endif()
