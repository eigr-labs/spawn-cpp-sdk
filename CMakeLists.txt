cmake_minimum_required(VERSION 3.1.2)
project(spawn-cpp-sdk CXX)

set(CMAKE_VERBOSE_MAKEFILE ON)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup(TARGETS)

find_package(Protobuf REQUIRED)

protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS resources/protos/eigr/actor.proto)
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS resources/protos/eigr/protocol.proto)

add_executable(${PROJECT_NAME} src/lib.cc ${PROTO_SRCS} ${PROTO_HDRS})
target_link_libraries(${PROJECT_NAME} PUBLIC CONAN_PKG::protobuf)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_BINARY_DIR})
set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD 11)

# TODO: Analyse use
#add_custom_target(proto_python ALL DEPENDS ${PROTO_PYS})