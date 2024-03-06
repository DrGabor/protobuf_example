project(hello_protobuf)
cmake_minimum_required(VERSION 3.8)

find_package(Protobuf REQUIRED)

# GOOGLE PROTOBUF
set(PROTOBUF_PROTOC_EXECUTABLE protoc)

file(GLOB_RECURSE proto_files "proto/*.proto")
message(STATUS "protofiles:" ${proto_files})
message(STATUS "project_source_dir:" ${PROJECT_SOURCE_DIR})
include(${PROJECT_SOURCE_DIR}/cmake/ProtoBuf.cmake)
message(STATUS "project_binary_dir:" ${PROJECT_BINARY_DIR})
set(proto_gen_folder "${PROJECT_BINARY_DIR}/proto")
protobuf_generate_cpp_py(${proto_gen_folder} proto_srcs proto_hdrs proto_python "${PROJECT_SOURCE_DIR}" "proto" ${proto_files})
message(STATUS "proto_gen_folder:" ${proto_gen_folder})
message(STATUS "protosrcs:" ${proto_srcs})
message(STATUS "proto_include_dir:" ${PROTOBUF_INCLUDE_DIR})
add_library(${PROJECT_NAME}_lib OBJECT ${proto_srcs})

include_directories(
    ${proto_gen_folder}
    ${PROTOBUF_INCLUDE_DIRS}
)

file(GLOB_RECURSE srcs "src/*.cpp")
message(STATUS "srcs:" ${srcs})
add_executable(${PROJECT_NAME} ${srcs})
target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}_lib ${PROTOBUF_LIBRARIES})

