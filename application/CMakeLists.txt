# =============================================================================
# Application
# =============================================================================

add_executable(Application
    main.cpp
)

target_include_directories(Application
    PRIVATE
        ${CMAKE_SOURCE_DIR}/application
        ${CMAKE_SOURCE_DIR}/library
)

target_link_libraries(Application
    PRIVATE
        Library
)
