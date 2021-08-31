# Build OBS-plugin
cmake_minimum_required(VERSION 3.16)

project (obs-backscrub)

find_package(LibObs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Threads REQUIRED)
add_subdirectory(backscrub) # Ideally this should be a "find_package" directive

set(obs-backscrub_SOURCES
        obs-backscrub.cpp)

add_library(obs-backscrub MODULE
        ${obs-backscrub_SOURCES})
# Remove the 'lib' prefix for this target
set_target_properties(obs-backscrub PROPERTIES PREFIX "")

target_link_libraries(obs-backscrub
        ${LIBOBS_LIBRARIES}
        libbackscrub
        ${OpenCV_LIBS})

include(GNUInstallDirs)
install(TARGETS obs-backscrub 
        LIBRARY	DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}/obs-plugins")
