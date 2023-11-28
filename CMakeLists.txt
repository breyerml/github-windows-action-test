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

if (WIN32)
    message(STATUS "WINDOWS")
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
    set(BUILD_SHARED_LIBS TRUE)
endif ()


find_package(Python3 REQUIRED COMPONENTS Interpreter Development)

include(${PROJECT_SOURCE_DIR}/cmake/check_python_libs.cmake)
set(TEST_REQUIRED_LIBS argparse humanize numpy) #sklearn
message(STATUS "Checking required Python3 libraries (${TEST_REQUIRED_LIBS}) to to generate the test file.")
check_python_libs("${TEST_REQUIRED_LIBS}" "or set PLSSVM_GENERATE_TEST_FILE to OFF.")

add_library(mylib SHARED test.cpp)

set(EXE_NAME prog)
add_executable(${EXE_NAME} main.cpp)

target_link_libraries(${EXE_NAME} PUBLIC mylib)

## setup testing wit GoogleTest
find_package(GTest 1.11.0 QUIET)
if (GTEST_FOUND)
    message(STATUS "Found package GTest.")
else ()
    message(STATUS "Couldn't find package GTest. Building from source...")
    set(PLSSVM_googletest_VERSION v1.14.0)
    # fetch testing framework GoogleTest
    include(FetchContent)
    FetchContent_Declare(googletest
            GIT_REPOSITORY https://github.com/google/googletest
            GIT_TAG ${PLSSVM_googletest_VERSION}
            GIT_SHALLOW TRUE
            QUIET
    )
    # For Windows: Prevent overriding the parent project's compiler/linker settings
    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
    FetchContent_GetProperties(googletest)
    if (NOT googletest_POPULATED)
        FetchContent_Populate(googletest)
        add_subdirectory(${googletest_SOURCE_DIR} ${googletest_SOURCE_DIR} EXCLUDE_FROM_ALL)
    endif ()
    message(STATUS "Installed GoogleTest version ${PLSSVM_googletest_VERSION}.")
endif ()



include(GoogleTest)

add_test(NAME foo COMMAND ${EXE_NAME})
