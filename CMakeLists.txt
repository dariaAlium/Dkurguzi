#Generate project VisualStudio with Win64!
cmake_minimum_required(VERSION 3.20)

project (Dkurguzi_Solution)
MESSAGE(${PROJECT_NAME}:)

MESSAGE("CMAKE_SYSTEM_NAME: " ${CMAKE_SYSTEM_NAME})
# The version number.

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "" FORCE)

set (BUILD_SHARED_LIBS ON )



add_subdirectory (libs/DynamicLib)
add_subdirectory (libs/StaticLib)
add_subdirectory (src/MainProject)

