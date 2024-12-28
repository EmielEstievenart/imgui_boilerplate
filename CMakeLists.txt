cmake_minimum_required(VERSION 3.15)

set(PROJECT_NAME imgui_boilerplate) 
project(${PROJECT_NAME} LANGUAGES CXX)


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

option(BP_BUILD_TESTS "Build tests" ON)
if(BP_BUILD_TESTS)
    add_subdirectory(tests)
endif()

set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})
set_target_properties(${PROJECT_NAME} PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY $(OutDir))


set_target_properties(imgui PROPERTIES FOLDER libraries)


