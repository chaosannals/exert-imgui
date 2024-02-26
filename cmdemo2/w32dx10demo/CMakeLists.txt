add_executable(w32dx10demo WIN32)

target_link_libraries(w32dx10demo imgui)

target_sources(
    w32dx10demo
    PRIVATE
    main.cpp
)
