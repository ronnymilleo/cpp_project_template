# =============================================================================
# Application
# =============================================================================

add_executable(${PROJECT_NAME}_App
    main.cpp
)

target_include_directories(${PROJECT_NAME}_App
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
)

target_link_libraries(${PROJECT_NAME}_App
    PRIVATE
        ${PROJECT_NAME}::Library
)
