project(example)
set(PADDLE_ROOT $ENV{PADDLE_ROOT} CACHE PATH "PaddlePaddle Root")
set(LIB_TYPE $ENV{LIB_TYPE} CACHE STRING "Choose the library type: static shared")
set(MATH_TYPE $ENV{LIB_TYPE} CACHE STRING "Choose the Math library type: openblas mkl")
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
    ${PADDLE_ROOT}/paddle/fluid/inference/libpaddle_fluid.so
    -lrt -ldl -lpthread)
else()
  if(MATH_TYPE STREQUAL "openblas")
    set(MATH_LIB ${PADDLE_ROOT}/third_party/install/openblas/lib/libopenblas.a)
  else()
    set(MATH_LIB ${PADDLE_ROOT}/third_party/install/mklml/lib/libmklml_intel.so)
  endif()
  set(CUDA_LIB $ENV{CUDA_LIB} CACHE PATH "CUDA Library Path")
  set(CUDNN_LIB $ENV{CUDNN_LIB} CACHE PATH "CUDNN Library Path")
  target_link_libraries(${PROJECT_NAME} 
    -Wl,--whole-archive
    ${PADDLE_ROOT}/paddle/fluid/inference/libpaddle_fluid.a
    -Wl,--no-whole-archive
    ${PADDLE_ROOT}/third_party/install/glog/lib/libglog.a
    ${PADDLE_ROOT}/third_party/install/gflags/lib/libgflags.a
    ${PADDLE_ROOT}/third_party/install/protobuf/lib/libprotobuf.a
    ${MATH_LIB}
    ${CUDA_LIB}/libcudart.so
    -lrt -ldl -lpthread)
endif()
