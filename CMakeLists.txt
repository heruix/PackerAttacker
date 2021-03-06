cmake_minimum_required (VERSION 3.6)
project (ThePackerAttacker)

# setup some required paths
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")

foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} UOUTPUTCONFIG )
    set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_${UOUTPUTCONFIG} "${CMAKE_CURRENT_SOURCE_DIR}/bin/${OUTPUTCONFIG}" )
    set( CMAKE_LIBRARY_OUTPUT_DIRECTORY_${UOUTPUTCONFIG} "${CMAKE_CURRENT_SOURCE_DIR}/bin/${OUTPUTCONFIG}" )
    set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${UOUTPUTCONFIG} "${CMAKE_CURRENT_SOURCE_DIR}/bin/${OUTPUTCONFIG}" )
endforeach( OUTPUTCONFIG CMAKE_CONFIGURATION_TYPES )


# set up deps and linkers and include stuff
include_directories ("src")

set (DEPS_DETOURS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/deps/detours")
include_directories ("${DEPS_DETOURS_DIR}/include")
link_libraries ("${DEPS_DETOURS_DIR}/lib.X86/detours.lib")

if (WIN32)
	link_libraries ("Psapi.lib")
endif()


# include our sub projects
add_subdirectory ("src")
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT PackerAttackerInject)