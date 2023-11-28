cmake_minimum_required(VERSION 3.22)

project(
  ModernCMakeExample
  VERSION 1.0
  LANGUAGES CXX)


option(PLSSVM_USE_GEMM "FOO" OFF)
option(PLSSVM_USE_FLOAT_AS_REAL_TYPE "BAR" OFF)


message(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
message(STATUS "PLSSVM_USE_GEMM: ${PLSSVM_USE_GEMM}")
message(STATUS "PLSSVM_USE_FLOAT_AS_REAL_TYPE: ${PLSSVM_USE_FLOAT_AS_REAL_TYPE}")

if (WIN32)
    message(STATUS "WINDOWS")
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
    set(BUILD_SHARED_LIBS TRUE)
endif ()

add_library(mylib SHARED test.cpp)
add_library(mylib2 SHARED test.cpp)

set(EXE_NAME prog)
add_executable(${EXE_NAME} main.cpp)

target_link_libraries(${EXE_NAME} PUBLIC mylib)
target_link_libraries(${EXE_NAME} PUBLIC mylib2)
