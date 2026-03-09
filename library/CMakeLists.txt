# =============================================================================
# Library
# =============================================================================

add_library(Library STATIC lib_main.cpp)

target_include_directories(Library
    PUBLIC
        ${CMAKE_SOURCE_DIR}/library
)

target_link_libraries(Library
    PUBLIC
        imgui
)
