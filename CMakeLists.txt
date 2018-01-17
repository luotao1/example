project(example)
set(PADDLE_ROOT $ENV{PADDLE_ROOT} CACHE PATH "PaddlePaddle Root")
cmake_minimum_required(VERSION 2.8)
add_executable(${PROJECT_NAME} example.cc)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)
include_directories(${PADDLE_ROOT}/third_party/gflags/include)
include_directories(${PADDLE_ROOT}/third_party/glog/include)
include_directories(${PADDLE_ROOT}/third_party/protobuf/include)
include_directories(${PADDLE_ROOT}/third_party/eigen3)
include_directories(${PADDLE_ROOT}/include)
target_link_libraries(${PROJECT_NAME} 
  ${PADDLE_ROOT}/lib/libpaddle_fluid_shared.so
  -lrt -ldl -lpthread)
