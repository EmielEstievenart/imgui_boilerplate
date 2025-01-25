project(imgui_base)

set(SOURCES
	code/imgui_base/imgui_impl_sdl3.cpp
	code/imgui_base/imgui_impl_vulkan.cpp
	code/imgui_base/vulkan_data.cpp
)

set(INCLUDES
	../includes/imgui_base/imgui_impl_sdl3.hpp
	../includes/imgui_base/imgui_impl_vulkan.hpp
	../includes/imgui_base/vulkan_data.hpp
)

add_library(imgui_base
	${SOURCES}
	${INCLUDES}
)

target_include_directories(imgui_base
	PUBLIC
	../includes/
	${SDL3_INCLUDE_DIRS}
	${Vulkan_INCLUDE_DIRS}
	PRIVATE
	./code
)

target_link_libraries(imgui_base PUBLIC
	imgui
	SDL3::SDL3-shared 
	${Vulkan_LIBRARIES}
)