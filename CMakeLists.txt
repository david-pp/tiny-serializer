cmake_minimum_required(VERSION 3.6)
project(tiny-serializer)

# protoc
set(PROTOC protoc)

# catch.hpp options
add_definitions(-DCATCH_PLATFORM_WINDOWS)

# CXX_FLAGS
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Werror -Wall -g")

set(SRC_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/include)
set(SRC_EXAMPLE ${CMAKE_CURRENT_SOURCE_DIR}/example)

include_directories(include)
include_directories(/usr/local/include)

link_directories(/usr/local/lib/)

execute_process(COMMAND ${PROTOC} -I=${SRC_INCLUDE} --cpp_out=${SRC_INCLUDE} ${SRC_INCLUDE}/archive.proto)
execute_process(COMMAND ${PROTOC} -I=${SRC_EXAMPLE} --cpp_out=${SRC_EXAMPLE} ${SRC_EXAMPLE}/player.proto)

add_library(tinyserializer STATIC ${SRC_INCLUDE}/archive.pb.h ${SRC_INCLUDE}/archive.pb.cc)

add_executable(demo_serialize example/demo_serialize.cpp example/player.pb.cc)
target_link_libraries(demo_serialize tinyserializer  protobuf)

add_executable(demo_serialize_dyn example/demo_serialize_dyn.cpp example/player.pb.cc)
target_link_libraries(demo_serialize_dyn tinyserializer protobuf)

add_executable(test_serialize test/test_serialize.cpp example/player.pb.cc)
target_link_libraries(test_serialize tinyserializer protobuf)
