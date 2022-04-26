cmake_minimum_required (VERSION 3.8)
 
project(MainProject)
 
set(SOURCE_FILES "MainProject.cpp")

find_library(DynamicLib DynamicLib.dll)
message(STATUS ${DynamicLib})
 
add_executable (MainProject ${SOURCE_FILES})
 
# Connecting the library, specify where to get the header files
include_directories("../../libs/StaticLib/src")
include_directories("../../libs/DynamicLib/src")
# And also we specify dependence on static library
target_link_libraries(MainProject StaticLib)
target_link_libraries(MainProject DynamicLib)

set_target_properties(MainProject
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
)