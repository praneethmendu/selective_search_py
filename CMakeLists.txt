cmake_minimum_required(VERSION 3.0)

project(selective_search)

execute_process(COMMAND python3.4-config --cflags  OUTPUT_VARIABLE PYTHON3_CFLAGS  OUTPUT_STRIP_TRAILING_WHITESPACE)
execute_process(COMMAND python3.4-config --ldflags OUTPUT_VARIABLE PYTHON3_LDFLAGS OUTPUT_STRIP_TRAILING_WHITESPACE)

set(SUPPRESS_WARNING "-Wall -Wno-unused-function -Wno-unused-variable -Wno-unused-local-typedefs")
set(CMAKE_CXX_FLAGS "-fPIC -O3 -std=c++11 ${PYTHON3_CFLAGS} ${SUPPRESS_WARNING}")
set(CMAKE_SHARED_LINKER_FLAGS "${PYTHON3_LDFLAGS} -shared -fPIC")

find_package(Boost 1.58 COMPONENTS "python" "numpy")
if(Boost_FOUND)
    include_directories(${CMAKE_SOURCE_DIR}/segment ${CMAKE_CURRENT_SOURCE_DIR} ${Boost_INCLUDE_DIRS})

    add_library(segment SHARED segment_py.cpp)
    target_link_libraries(segment ${Boost_LIBRARIES})
    set_target_properties(segment PROPERTIES PREFIX "" SUFFIX ".so")
ELSEIF(NOT Boost_FOUND)
    message(FATAL_ERROR "Unable to find Boost.")
endif()
