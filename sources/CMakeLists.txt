set(SOURCES
	code/cpp_boilerplate/main.cpp
)

set(RESOURCES
	resources/some_data.txt
	resources/test.jpg
)

set(INCLUDES

)

add_executable(${PROJECT_NAME}
	${SOURCES}
	${RESOURCES}
	${INCLUDES}
)

target_link_libraries(${PROJECT_NAME} PRIVATE 
	imgui
	imgui_base
	imgui_helpers
	SDL3::SDL3-shared 
	${Vulkan_LIBRARIES}
	stb	
)

target_include_directories(${PROJECT_NAME}
	PUBLIC
	../includes
	PRIVATE
	code
	${SDL3_INCLUDE_DIRS}
	${Vulkan_INCLUDE_DIRS}
)

if(RESOURCES)
	set_target_properties(${PROJECT_NAME}
		PROPERTIES
		RESOURCE "${RESOURCES}"
	)

	add_custom_command(
		TARGET ${PROJECT_NAME}
		POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E make_directory
				$<TARGET_FILE_DIR:cpp_boilerplate>/resources
		COMMAND ${CMAKE_COMMAND} -E copy_directory
			${CMAKE_CURRENT_SOURCE_DIR}/resources
			$<TARGET_FILE_DIR:cpp_boilerplate>/resources
	)
endif()

if(WIN32)
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND "${CMAKE_COMMAND}" -E copy $<TARGET_FILE:SDL3::SDL3-shared> $<TARGET_FILE_DIR:${PROJECT_NAME}>
        VERBATIM
    )
endif()

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} FILES ${SOURCES} ${RESOURCES})

