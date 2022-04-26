

################################
########## DynamicLib.dll ##########
################################
project (DynamicLib)
MESSAGE(${PROJECT_NAME})


set(SOURCE_FILES "src/DynamicLib.cpp")
set(HEADER_FILES "src/DynamicLib.h")
 
# We declare the project as a static library and add all the source code files to it.
add_library(DynamicLib SHARED ${HEADER_FILES} ${SOURCE_FILES})

install( TARGETS DynamicLib
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} )

set_target_properties(DynamicLib
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}
)

################# DynamicLib LIB END #########################