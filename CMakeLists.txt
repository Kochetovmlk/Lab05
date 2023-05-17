cmake_minimum_required(VERSION 3.12)

project(banking)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/CMakeModules)

add_library(banking STATIC
    banking.cpp
)

add_executable(main main.cpp)
target_link_libraries(main banking)
