cmake_minimum_required(VERSION 3.1.2)

set(PROJECT_NAME APart)
project(${PROJECT_NAME} VERSION 5.4.3)

if ( "x${CMAKE_CONFIGURATION_TYPES}" STREQUAL "xRelease")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /Zi /MD")
	set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /DEBUG /OPT:REF /OPT:ICF")
else()
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MDd")
endif()

add_definitions("-DUNICODE" "-D_UNICODE")

file(GLOB_RECURSE ${PROJECT_NAME}_SOURCE
		Src/*.cpp)

add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME}_SOURCE})

target_include_directories(
	${PROJECT_NAME}
	PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/Export
	${CMAKE_CURRENT_SOURCE_DIR}/Src
	${CMAKE_INCLUDE_CURRENT_DIR}
)

if (CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")
	include(MSVC_Structed.cmake)
	assign_source_group(${${PROJECT_NAME}_SOURCE})
endif()


file(GLOB_RECURSE ${PROJECT_NAME}_Interface
		Export/*)
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER ${${PROJECT_NAME}_Interface})



install(TARGETS ${PROJECT_NAME} 
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    PUBLIC_HEADER DESTINATION include)

include(CMakePackageConfigHelpers)
	write_basic_package_version_file(
		"${PROJECT_NAME}ConfigVersion.cmake"
		VERSION ${${PROJECT_NAME}_VERSION}
		COMPATIBILITY AnyNewerVersion)	

install(EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    NAMESPACE ${PROJECT_NAME}::
	DESTINATION lib/cmake/${PROJECT_NAME} )
	

install(FILES "${PROJECT_NAME}Config.cmake" "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
    DESTINATION lib/cmake/${PROJECT_NAME})