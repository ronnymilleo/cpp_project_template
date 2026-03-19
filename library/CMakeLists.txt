# =============================================================================
# Library
# =============================================================================

add_library(${PROJECT_NAME}_Library STATIC lib_main.cpp)
add_library(${PROJECT_NAME}::Library ALIAS ${PROJECT_NAME}_Library)

target_include_directories(${PROJECT_NAME}_Library
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(${PROJECT_NAME}_Library
    PUBLIC
        imgui
)
