cmake_minimum_required(VERSION 3.25)

project(
  ModernCMakeExample
  VERSION 1.0
  LANGUAGES CXX)


option(PLSSVM_USE_GEMM "FOO" OFF)
option(PLSSVM_USE_FLOAT_AS_REAL_TYPE "BAR" OFF)


message(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
message(STATUS "PLSSVM_USE_GEMM: ${PLSSVM_USE_GEMM}")
message(STATUS "PLSSVM_USE_FLOAT_AS_REAL_TYPE: ${PLSSVM_USE_FLOAT_AS_REAL_TYPE}")


find_package(Python3 REQUIRED COMPONENTS Interpreter Development)

include(${PROJECT_SOURCE_DIR}/cmake/check_python_libs.cmake)
set(TEST_REQUIRED_LIBS argparse humanize numpy) #sklearn
message(STATUS "Checking required Python3 libraries (${TEST_REQUIRED_LIBS}) to to generate the test file.")
check_python_libs("${TEST_REQUIRED_LIBS}" "or set PLSSVM_GENERATE_TEST_FILE to OFF.")


add_executable(prog main.cpp)
