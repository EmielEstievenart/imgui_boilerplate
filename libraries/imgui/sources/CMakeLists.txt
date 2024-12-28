project(imgui)

set(SOURCES
	code/imgui/imconfig.h
	code/imgui/imgui.cpp
	code/imgui/imgui.h
	code/imgui/imgui_demo.cpp
	code/imgui/imgui_draw.cpp
	code/imgui/imgui_internal.h
	code/imgui/imgui_tables.cpp
	code/imgui/imgui_widgets.cpp
	code/imgui/imstb_rectpack.h
	code/imgui/imstb_textedit.h
	code/imgui/imstb_truetype.h
)

add_library(imgui
	${SOURCES}
)

target_include_directories(imgui
	PUBLIC
	./code
	PRIVATE
	./code/imgui
)