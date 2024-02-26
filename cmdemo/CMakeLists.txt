cmake_minimum_required(VERSION 3.24)

include(FetchContent)

project(cmdemo)

FetchContent_Declare(
    imgui
    GIT_REPOSITORY git@github.com:ocornut/imgui.git
    GIT_TAG v1.90.4
    GIT_SHALLOW ON
    GIT_PROGRESS ON
)

FetchContent_GetProperties(imgui)


# 目前只 适配 win32 dx10 版本
# TODO 并非这样混合，需要再设计
set(IMGUI_BACKEND "win32" CACHE STRING "imGui backend.")
set(IMGUI_BACKEND2 "dx10" CACHE STRING "imGui backend2.")
set_property(
    CACHE IMGUI_BACKEND
    PROPERTY STRINGS
    "allegro5"
    "android"
    "glfw"
    "glut"
    "metal"
    "osx"
    "sdl2"
    "sdl3"
    "vulkan"
    "wgpu"
    "win32"
)
set_property(
    CACHE IMGUI_BACKEND2
    PROPERTY STRINGS
    "opengl2"
    "opengl3"
    "dx9"
    "dx10"
    "dx11"
    "dx12"
    "sdlrenderer2"
    "sdlrenderer3"
    "opengl3_loader"
)

if (NOT imgui_POPULATED)
    message(STATUS "populate imgui")
    FetchContent_Populate(imgui)
    message(STATUS "imgui src: ${imgui_SOURCE_DIR}")

    add_library(imgui STATIC)
    set_target_properties(imgui PROPERTIES CXX_STANDARD 20)

    file(GLOB IMGUI_SRC "${imgui_SOURCE_DIR}/*.cpp")
    set(IMGUI_BACKEND_SRC "${imgui_SOURCE_DIR}/backends/imgui_impl_${IMGUI_BACKEND}.cpp")
    set(IMGUI_BACKEND2_SRC "${imgui_SOURCE_DIR}/backends/imgui_impl_${IMGUI_BACKEND2}.cpp")

    message(STATUS "imgui cpp: ${IMGUI_BACKEND_SRC} ${IMGUI_BACKEND2_SRC} ${IMGUI_SRC}")
    target_sources(
        imgui
        PRIVATE
        ${IMGUI_SRC}
        ${IMGUI_BACKEND_SRC}
        ${IMGUI_BACKEND2_SRC}
    )

    if (${IMGUI_BACKEND2} STREQUAL "dx10")
        message(STATUS "imgui backend2: ${IMGUI_BACKEND2_SRC}")
        set(IMGUI_BACKEND_LIBS "d3d10.lib" "d3dcompiler.lib")
    endif()

    message(STATUS "imgui backend libs: ${IMGUI_BACKEND_LIBS}")
    target_link_libraries(imgui ${IMGUI_BACKEND_LIBS})

    target_include_directories(
        imgui
        PUBLIC
        "${imgui_SOURCE_DIR}"
    )
endif()


add_subdirectory(simple)
