cmake_minimum_required(VERSION 2.8.9)
project (project)
set(CMAKE_CXX_STANDARD 11)

find_library(LIB libEngineLibrary.so)
message(STATUS ${LIB})

add_executable(project src/main.cpp)

target_link_libraries(project PRIVATE ${LIB})