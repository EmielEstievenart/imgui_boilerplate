project(imgui_helpers)

set(SOURCES
	code/imgui_helpers/image_helper.cpp
)

set(INCLUDES
	../includes/imgui_helpers/image_helper.hpp
)

add_library(imgui_helpers
	${SOURCES}
	${INCLUDES}
)

target_include_directories(imgui_helpers
	PUBLIC
	../includes/
	PRIVATE
	./code
)

target_link_libraries(imgui_helpers PRIVATE stb imgui_base)