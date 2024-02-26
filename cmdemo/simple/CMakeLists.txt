add_executable(simple WIN32)

target_link_libraries(simple imgui)

target_sources(
	simple
	PRIVATE
	main.cpp
)