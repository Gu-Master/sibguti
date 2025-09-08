cmake_minimum_required(VERSION 3.10)
project(MyZMQApp C)

set(CMAKE_C_STANDARD 99)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ZMQ REQUIRED libzmq)
pkg_check_modules(CZMQ REQUIRED libczmq)

include_directories(${ZMQ_INCLUDE_DIRS} ${CZMQ_INCLUDE_DIRS} include)

add_executable(server src/server.c)
target_link_libraries(server ${ZMQ_LIBRARIES} ${CZMQ_LIBRARIES})

add_executable(client src/client.c)
target_link_libraries(client ${ZMQ_LIBRARIES} ${CZMQ_LIBRARIES})
