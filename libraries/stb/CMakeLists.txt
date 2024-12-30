project(stb)

add_library(stb INTERFACE
	includes/stb/stb_image.h
)

target_include_directories(stb
	INTERFACE
	includes
)