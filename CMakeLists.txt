cmake_minimum_required(VERSION 3.0)
project(libcbor)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules/")
include(CTest)

SET(CBOR_VERSION_MAJOR "0")
SET(CBOR_VERSION_MINOR "10")
SET(CBOR_VERSION_PATCH "2")
SET(CBOR_VERSION ${CBOR_VERSION_MAJOR}.${CBOR_VERSION_MINOR}.${CBOR_VERSION_PATCH})

option(CBOR_PRETTY_PRINTER "Include a pretty-printing routine" ON)
option(CBOR_WITH_TESTS "[TEST] Build unit tests (requires CMocka)" OFF)
option(CBOR_WITH_EXAMPLES "Build examples" ON)
option(CBOR_HUGE_FUZZ "[TEST] Fuzz through 8GB of data in the test. Do not use with memory instrumentation!" OFF)
option(CBOR_SANE_MALLOC "[TEST] Assume that malloc will not allocate multi-GB blocks. Tests only, platform specific" OFF)
option(CBOR_PRINT_FUZZ "[TEST] Print the fuzzer input" OFF)

set(CBOR_BUFFER_GROWTH "2" CACHE STRING "Factor for buffer growth & shrinking")
set(CBOR_MAX_STACK_SIZE "2048" CACHE STRING "maximum size for decoding context stack")


if(CBOR_WITH_TESTS)
    add_definitions(-DCBOR_WITH_TESTS)
endif(CBOR_WITH_TESTS)

enable_testing()

set(CTEST_MEMORYCHECK_COMMAND "/usr/bin/valgrind")
set(MEMORYCHECK_COMMAND_OPTIONS "--tool=memcheck --track-origins=yes --leak-check=full --error-exitcode=1")

add_subdirectory(src)

if (CBOR_WITH_TESTS)
    add_subdirectory(test)
endif (CBOR_WITH_TESTS)

if (CBOR_WITH_EXAMPLES)
    add_subdirectory(examples)
endif (CBOR_WITH_EXAMPLES)
