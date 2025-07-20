cmake_minimum_required(VERSION 3.22)
project(tc_server_protocol)

set(CMAKE_CXX_STANDARD 20)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${GR_PROJECT_PATH}/tc_3rdparty/asio2/include)
include_directories(${GR_PROJECT_PATH}/tc_3rdparty/asio2/3rd)

if(ANDROID)
    find_package(Protobuf REQUIRED)
    message("==> Protobuf libraries: ${Protobuf_LIBRARIES}")
    ### relay message
    protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS relay_message.proto)
    add_library(tc_relay_message
            ${CMAKE_CURRENT_BINARY_DIR}/relay_message.pb.cc
    )
    target_link_libraries(tc_relay_message protobuf::libprotoc protobuf::libprotobuf protobuf::libprotobuf-lite)

elseif(WIN32 OR APPLE OR UNIX)

    ### relay message
    protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS relay_message.proto)
    add_library(tc_relay_message
            ${CMAKE_CURRENT_BINARY_DIR}/relay_message.pb.cc
    )
    target_link_libraries(tc_relay_message libprotobuf)
endif()