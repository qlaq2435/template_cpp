cmake_minimum_required(VERSION 3.0.0)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -target x86_64-w64-windows-gnu")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -Wall -std=c++17 -target x86_64-w64-windows-gnu")

project (hello)

set(CMAKE_C_COMPILER "clang")
set(CMAKE_CXX_COMPILER "clang++")

include(CTest)
enable_testing()
include_directories(include/hello) 


aux_source_directory(${PROJECT_SOURCE_DIR}/src  src_dirs)
list(APPEND src_dirs ${PROJECT_SOURCE_DIR}/src/hello/hello.cpp)


# 编译条件判断
if(CMAKE_BUILD_TYPE AND (CMAKE_BUILD_TYPE STREQUAL "Debug"))
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -Wall -O0")
    message("Debug mode:${CMAKE_C_FLAGS_DEBUG}")
    add_executable(debug ${src_dirs})

elseif(CMAKE_BUILD_TYPE AND (CMAKE_BUILD_TYPE STREQUAL "Release"))
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -Wall -O3")
    message("Release mode:${CMAKE_C_FLAGS_RELEASE}")
    add_executable(release ${src_dirs})
else()
    message("no_specified:${CMAKE_BUILD_TYPE}")
    message("no_specified:${CMAKE_C_FLAGS_RELEASE}")
    add_executable(no_specified ${src_dirs})
endif()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})

include(CPack)
