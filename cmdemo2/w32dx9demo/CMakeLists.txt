add_executable(w32dx9demo)

target_link_libraries(w32dx9demo imgui)

target_sources(
    w32dx9demo
    PRIVATE
    main.cpp
)
