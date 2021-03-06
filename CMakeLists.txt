cmake_minimum_required(VERSION 2.6)
project(tuo)
#find_package(PythonExtensions )
# This file is part of MXE. See LICENSE.md for licensing information.

# partial module - included by src/cmake/CMakeLists.txt

#set(TGT test-${PKG}-cmake)
set( CMAKE_CXX_STANDARD 11 )
set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror -Ofast")
#set( BUILD_SHARED_LIBRARIES OFF )
set( Boost_USE_STATIC_LIBS ON )
#set( BOOST_USE_STATIC_RUNTIME OFF )
#set( CMAKE_EXE_LINKER_FLAGS "-static")
add_definitions(-DTYRANT_OPTIMIZER_VERSION="${VERSION}" ${DEBUG} -DNQUEST)



file(GLOB TUO_SOURCES "tyrant_optimize/*.cpp")

enable_language(CXX)
add_executable(tuo ${TUO_SOURCES})

option(USE_OPENMP "Use OpenMP instead of Boost" OFF)
if(USE_OPENMP)
# src: https://stackoverflow.com/a/12404666
find_package(OpenMP REQUIRED)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()
endif()

find_package(Boost COMPONENTS system thread filesystem regex timer REQUIRED)
#set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lboost_system -lboost_thread -lboost_filesystem -lboost_regex -lboost_timer -lpthread")
#message(WARNING ${Boost_LIBRARIES})

target_link_libraries(tuo ${Boost_LIBRARIES} )
install(TARGETS tuo LIBRARY DESTINATION tyrant_optimize)
#
#check_dynamic_lookup("EXE" "STATIC" TMP)
#message( WARNING ${TMP})
