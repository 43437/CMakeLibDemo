cmake_minimum_required(VERSION 3.1.2)

set(CMAKE_CXX_STANDARD 11)

set(PROJECT_NAME testAPart)
project(${PROJECT_NAME})

if (CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")
	if ( "x${CMAKE_CONFIGURATION_TYPES}" STREQUAL "xRelease")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /Zi /MD")
	set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /DEBUG /OPT:REF /OPT:ICF")
	else()
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MDd")
	endif()

	add_definitions("-DUNICODE" "-D_UNICODE")
else()
endif()

set(APart_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../APart/install/lib/cmake/APart)
find_package(APart)

file(GLOB_RECURSE ${PROJECT_NAME}_SOURCE
		Src/*.cpp)

add_executable(${PROJECT_NAME} ${${PROJECT_NAME}_SOURCE})
target_link_libraries( ${PROJECT_NAME} APart::APart)
# get_target_property(APart_dll APart::APart IMPORTED_LOCATION_RELEASE)

target_include_directories(
	${PROJECT_NAME}
	PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}
	${APart_DIR}/../../../include/
	${CMAKE_INCLUDE_CURRENT_DIR}
)