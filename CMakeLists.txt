cmake_minimum_required(VERSION 3.15)

set(PROJECT_NAME cpp_boilerplate) 
project(${PROJECT_NAME} LANGUAGES CXX)

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

#This is required to use the folders in Visual Studio

add_subdirectory(sources)
add_subdirectory(libraries)

option(USE_UNIT_TESTS "Build tests" ON)
if(USE_UNIT_TESTS)
    add_subdirectory(tests/unit_tests)
endif()

include(cppcheck REQUIRED)
target_add_cppcheck(TARGET ${PROJECT_NAME} NUMBER_OF_THREADS 4 SUPPRESSIONS "missingIncludeSystem" EXTRA_COMMANDS "--template=gcc" "--verbose" "--inline-suppr" "--inconclusive" "--check-level=exhaustive" "--enable=all" "--inconclusive" "--force" ${GOOGLETEST_IGNORE_DIR} "--checkers-report=cppcheck-report.txt")

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY $(OutDir))


