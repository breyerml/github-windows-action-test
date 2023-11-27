project(
  ModernCMakeExample
  VERSION 1.0
  LANGUAGES CXX)


find_package(Python3 REQUIRED COMPONENTS Interpreter Development NumPy)

include(${PROJECT_SOURCE_DIR}/cmake/check_python_libs.cmake)
set(TEST_REQUIRED_LIBS argparse sklearn humanize)
message(STATUS "Checking required Python3 libraries (${TEST_REQUIRED_LIBS}) to to generate the test file.")
check_python_libs("${TEST_REQUIRED_LIBS}" "or set PLSSVM_GENERATE_TEST_FILE to OFF.")


add_executable(prog main.cpp)
