#Generate project VisualStudio with Win64!
cmake_minimum_required(VERSION 3.11)

################################
########## StaticLib.lib ##########
################################
project (StaticLib)
MESSAGE(${PROJECT_NAME})


set(SOURCE_FILES "src/StaticLib.cpp")
set(HEADER_FILES "src/StaticLib.h")

set (CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON )
 
# We declare the project as a static library and add all the source code files to it.
add_library(StaticLib STATIC ${HEADER_FILES} ${SOURCE_FILES})

set_target_properties(StaticLib
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
)

################# StaticLib LIB END #########################
