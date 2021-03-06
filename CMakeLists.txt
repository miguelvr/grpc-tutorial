cmake_minimum_required(VERSION 3.7)
project(grpc_tutorial)

set(CMAKE_CXX_STANDARD 17)

# Find Dependencies
find_package(Protobuf REQUIRED)
find_package(gRPC REQUIRED)

# Protobuf stuff
set(PROTO_PATH ${CMAKE_CURRENT_SOURCE_DIR}/protos)
set(PROTOS ${PROTO_PATH}/helloworld.proto)

set(PROTO_GENERATED_DIR "${CMAKE_BINARY_DIR}/generated")
file(MAKE_DIRECTORY ${PROTO_GENERATED_DIR})

# cmake modules
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})
include(protobuf)   # includes protobuf helper function (protobuf_generate_cpp)
include(grpc)       # includes grpc helper function (grpc_generate_cpp)

# generate protobuf related files with protoc
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${PROTO_GENERATED_DIR} ${PROTOS})
grpc_generate_cpp(GRPC_SRCS GRPC_HDRS ${PROTO_GENERATED_DIR} ${PROTOS})

set(PROTO_GENERATED_FILES
        ${PROTO_SRCS}
        ${PROTO_HDRS}
        ${GRPC_SRCS}
        ${GRPC_HDRS})

# TARGETS
#############################################

# Building server
add_executable(grpc_server
        src/server/server.cpp
        ${PROTO_GENERATED_FILES})

target_link_libraries(grpc_server
        gRPC::grpc++_reflection
        protobuf::libprotobuf)

target_include_directories(grpc_server PUBLIC ${PROTO_GENERATED_DIR})

# Building client
add_executable(grpc_client
        src/client/client.cpp
        ${PROTO_GENERATED_FILES})

target_link_libraries(grpc_client
        gRPC::grpc++_reflection
        protobuf::libprotobuf)

target_include_directories(grpc_client PUBLIC ${PROTO_GENERATED_DIR})
