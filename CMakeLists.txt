# Build OBS-plugin
cmake_minimum_required(VERSION 3.16)

project (obs-backscrub)

find_package(LibObs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Threads REQUIRED)

if(NOT DEFINED BACKSCRUB_BUILD_DIR)
        set(BACKSCRUB_BUILD_DIR ../backscrub/build)
endif()

if (NOT DEFINED LIBBACKSCRUB_INCLUDE_DIR)
        set(LIBBACKSCRUB_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include)
endif()

# Pull in backscrub and dependencies
include(${BACKSCRUB_BUILD_DIR}/BackscrubTargets.cmake)

set(obs-backscrub_SOURCES
        obs-backscrub.cpp)

add_library(obs-backscrub MODULE
        ${obs-backscrub_SOURCES})
# Remove the 'lib' prefix for this target
set_target_properties(obs-backscrub PROPERTIES PREFIX "")

target_include_directories(obs-backscrub
        PUBLIC ${LIBBACKSCRUB_INCLUDE_DIR})

target_link_libraries(obs-backscrub
        ${LIBOBS_LIBRARIES}
        backscrub
        ${OpenCV_LIBS})

include(GNUInstallDirs)
install(TARGETS obs-backscrub 
        LIBRARY	DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}/obs-plugins")
