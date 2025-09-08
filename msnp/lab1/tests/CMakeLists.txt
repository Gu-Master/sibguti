cmake_minimum_required(VERSION 3.10)
project(MyZMQApp C)

set(CMAKE_C_STANDARD 99)

find_package(PkgConfig REQUIRED)
pkg_check_modules(ZMQ REQUIRED libzmq)
pkg_check_modules(CZMQ REQUIRED libczmq)


include_directories(${ZMQ_INCLUDE_DIRS} ${CZMQ_INCLUDE_DIRS})


add_executable(server server.c)

target_link_libraries(server ${ZMQ_LIBRARIES} ${CZMQ_LIBRARIES})


add_executable(client client.c)

target_link_libraries(client ${ZMQ_LIBRARIES} ${CZMQ_LIBRARIES})