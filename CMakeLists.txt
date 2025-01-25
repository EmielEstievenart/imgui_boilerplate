cmake_minimum_required(VERSION 3.15)

set(PROJECT_NAME cpp_boilerplate) 
project(${PROJECT_NAME} LANGUAGES C CXX)

set(CMAKE_HELPER_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

list(PREPEND CMAKE_MODULE_PATH "${CMAKE_HELPER_DIRECTORY}")

# GoogleTest requires at least C++14
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#Find SDL3
find_package(SDL3 REQUIRED CONFIG REQUIRED COMPONENTS SDL3-shared)
if(SDL3_FOUND)
    message(STATUS "Using SDL3 via find_package")
endif()

#Find vulkan
include(FindVulkan)
if(Vulkan_FOUND)
	message("Found VULKAN")
else()
	message("Didn't find vulkan")
endif()


add_subdirectory(libraries)
add_subdirectory(sources)

option(USE_UNIT_TESTS "Build tests" ON)
if(USE_UNIT_TESTS)
    add_subdirectory(tests/unit_tests)
endif()

message("The vulkan include dirs are ${Vulkan_INCLUDE_DIRS}")

include(cppcheck REQUIRED)
target_add_cppcheck(TARGET ${PROJECT_NAME}
	NUMBER_OF_THREADS 
		4 
	SUPPRESSIONS 
		"missingIncludeSystem" 
	DEFINES
		#Add a define to force only 1 configuration. 
		DEBUG=1 
	SUPPRESSIONS 
		"*:*libraries*"
		"*:*main.cpp"
	IGNORE_FOLDERS 
		"${CMAKE_SOURCE_DIR}/libraries" 
		"${SDL3_INCLUDE_DIRS}" 
		"${Vulkan_INCLUDE_DIRS}" 
	EXTRA_COMMANDS
		"--template=gcc" 
		"--verbose" 
		"--inline-suppr" 
		"--inconclusive" 
		"--check-level=exhaustive" 
		"--enable=all" 
		"--inconclusive" 
		"--file-filter=*sources/code/${PROJECT_NAME}/*" 
		"--checkers-report=cppcheck-report.txt" 
)

#This is required to use the folders in Visual Studio
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY $(OutDir))


