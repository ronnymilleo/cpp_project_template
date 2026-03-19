# =============================================================================
# Application
# =============================================================================
# {{PROJECT_DESCRIPTION}}
#
# Author: Ronny Milleo
# License: See LICENSE file in project root
# =============================================================================

cmake_minimum_required(VERSION 3.28)

# Project definition with version and metadata
project(Application
    VERSION 0.1.0
    DESCRIPTION "{{PROJECT_DESCRIPTION}}"
    LANGUAGES CXX  # Change to C for C projects
)

# =============================================================================
# C++ Standard and Compiler Requirements
# =============================================================================

# Require modern C++ standard
set(CMAKE_CXX_STANDARD 23)  # Adjust as needed (17, 20, 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)  # Disable compiler-specific extensions

# For C projects, use:
# set(CMAKE_C_STANDARD 99)  # or 11, 17, 23
# set(CMAKE_C_STANDARD_REQUIRED ON)
# set(CMAKE_C_EXTENSIONS OFF)

# =============================================================================
# Default Build Type
# =============================================================================

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to 'Release' as none was specified.")
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# =============================================================================
# Compiler Configuration
# =============================================================================

# Enable compile commands export for IDE support (clangd, etc.)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Set output directories for better organization
string(TOLOWER "${CMAKE_BUILD_TYPE}" BUILD_TYPE_LOWER)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/out/${BUILD_TYPE_LOWER}/bin")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/out/${BUILD_TYPE_LOWER}/lib")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/out/${BUILD_TYPE_LOWER}/lib")

# =============================================================================
# Formatting Target (clang-format)
# =============================================================================

find_program(CLANG_FORMAT NAMES clang-format)
if(CLANG_FORMAT)
    # Collect all source files for formatting
    file(GLOB_RECURSE ALL_SOURCE_FILES
        ${CMAKE_CURRENT_SOURCE_DIR}/application/*.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/application/*.h
        ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
        ${CMAKE_CURRENT_SOURCE_DIR}/include/*.h
        ${CMAKE_CURRENT_SOURCE_DIR}/include/*.hpp
        ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/tests/*.c
        ${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cc
        ${CMAKE_CURRENT_SOURCE_DIR}/examples/*.c
    )

    add_custom_target(format
        COMMAND ${CLANG_FORMAT} -i ${ALL_SOURCE_FILES}
        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        COMMENT "Formatting source code with clang-format"
        VERBATIM
    )
else()
    message(STATUS "clang-format not found; skipping automatic source formatting")
    add_custom_target(format
        COMMAND ${CMAKE_COMMAND} -E echo "clang-format not found, skipping formatting"
    )
endif()

# =============================================================================
# Dependencies
# =============================================================================

add_subdirectory(vendor)

# =============================================================================
# Main Library Target
# =============================================================================

add_subdirectory(library)

# =============================================================================
# Main Executable Target
# =============================================================================

add_subdirectory(application)

# =============================================================================
# Testing
# =============================================================================

option(${PROJECT_NAME}_BUILD_TESTS "Build tests" OFF)
if(${PROJECT_NAME}_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

# =============================================================================
# Post-Build Commands
# =============================================================================

add_custom_target(copy_compile_commands
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${CMAKE_CURRENT_BINARY_DIR}/compile_commands.json"
        "${CMAKE_CURRENT_SOURCE_DIR}/compile_commands.json"
    COMMENT "Copying compile_commands.json to project root"
    VERBATIM
)

add_dependencies(${PROJECT_NAME}_App copy_compile_commands)

# =============================================================================
# Project Summary
# =============================================================================

message(STATUS "")
message(STATUS "=== Application Configuration ===")
message(STATUS "Project:              ${PROJECT_NAME}")
message(STATUS "Version:              ${PROJECT_VERSION}")
message(STATUS "Build type:           ${CMAKE_BUILD_TYPE}")
message(STATUS "C++ standard:         ${CMAKE_CXX_STANDARD}")
message(STATUS "Compiler:             ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")
message(STATUS "=========================================")
message(STATUS "")

# =============================================================================
# End of file
# =============================================================================
