# =============================================================================
# Dependencies
# =============================================================================

# =============================================================================
# GLFW
# =============================================================================

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/glfw ${CMAKE_CURRENT_BINARY_DIR}/glfw)

# Ensure GLFW is built as a static library
set_target_properties(glfw PROPERTIES
    POSITION_INDEPENDENT_CODE ON
)

# =============================================================================
# Create imported target for OpenGL
# =============================================================================

find_package(OpenGL REQUIRED)

add_library(OpenGL INTERFACE IMPORTED)
target_include_directories(OpenGL INTERFACE ${OPENGL_INCLUDE_DIRS})
target_link_libraries(OpenGL INTERFACE ${OPENGL_LIBRARIES})

# =============================================================================
# imgui
# =============================================================================

add_library(imgui STATIC
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_demo.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_draw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_tables.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/imgui_widgets.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends/imgui_impl_glfw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends/imgui_impl_opengl3.cpp
)

target_include_directories(imgui PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui
    ${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends
)

target_link_libraries(imgui PUBLIC
    OpenGL
    glfw
)

# Set OpenGL version for ImGui
target_compile_definitions(imgui PUBLIC
    IMGUI_IMPL_OPENGL_LOADER_IMGL3W
)

target_compile_options(imgui PRIVATE
    -w
)
