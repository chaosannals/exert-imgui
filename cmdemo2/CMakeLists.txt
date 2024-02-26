cmake_minimum_required(VERSION 3.24)

include(FetchContent)

project(cmdemo2)

FetchContent_Declare(
    imgui
    GIT_REPOSITORY git@github.com:ocornut/imgui.git
    GIT_TAG v1.90.4
    GIT_SHALLOW ON
    GIT_PROGRESS ON
)

FetchContent_GetProperties(imgui)

set(IMGUI_BACKEND "win32_directx10" CACHE STRING "imGui backend.")
set_property(
    CACHE IMGUI_BACKEND
    PROPERTY STRINGS
    "allegro5"
    "android_opengl3"
    "apple_metal"
    "apple_opengl2"
    "emscripten_wgpu"
    "glfw_metal"
    "glfw_opengl2"
    "glfw_opengl3"
    "glfw_vulkan"
    "glut_opengl2"
    "null"
    "sdl2_directx11"
    "sdl2_metal"
    "sdl2_opengl2"
    "sdl2_opengl3"
    "sdl2_sdlrenderer2"
    "sdl2_vulkan"
    "sdl3_opengl3"
    "sdl3_sdlrenderer3"
    "win32_directx9"
    "win32_directx10"
    "win32_directx11"
    "win32_directx12"
    "win32_opengl3"
)

if (NOT imgui_POPULATED)
    message(STATUS "populate imgui")
    FetchContent_Populate(imgui)
    message(STATUS "imgui src: ${imgui_SOURCE_DIR}")

    add_library(imgui STATIC)
    set_target_properties(imgui PROPERTIES CXX_STANDARD 20)

    file(GLOB IMGUI_SRC "${imgui_SOURCE_DIR}/*.cpp")

    if (${IMGUI_BACKEND} STREQUAL "win32_directx9")
        set(
            IMGUI_BACKEND_SRC
            "${imgui_SOURCE_DIR}/backends/imgui_impl_win32.cpp"
            "${imgui_SOURCE_DIR}/backends/imgui_impl_dx9.cpp"
        )
        set(IMGUI_BACKEND_LIBS "d3d9.lib")
    elseif (${IMGUI_BACKEND} STREQUAL "win32_directx10")
        set(
            IMGUI_BACKEND_SRC
            "${imgui_SOURCE_DIR}/backends/imgui_impl_win32.cpp"
            "${imgui_SOURCE_DIR}/backends/imgui_impl_dx10.cpp"
        )
        set(IMGUI_BACKEND_LIBS "d3d10.lib" "d3dcompiler.lib")
    endif()

    message(STATUS "imgui cpp: ${IMGUI_BACKEND_SRC} ${IMGUI_SRC}")
    target_sources(
        imgui
        PRIVATE
        ${IMGUI_SRC}
        ${IMGUI_BACKEND_SRC}
    )

    message(STATUS "imgui libs: ${IMGUI_BACKEND_LIBS}")
    target_link_libraries(imgui ${IMGUI_BACKEND_LIBS})

    target_include_directories(
        imgui
        PUBLIC
        "${imgui_SOURCE_DIR}"
    )
endif()

if (${IMGUI_BACKEND} STREQUAL "win32_directx9")
    add_subdirectory(w32dx9demo)
elseif (${IMGUI_BACKEND} STREQUAL "win32_directx10")
    add_subdirectory(w32dx10demo)
endif()
