#-----------------------------------------------------------------------------------------------------------------------
# Information
#-----------------------------------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 4.3.0)
project("cpp_map"
        VERSION 0.1.0
        DESCRIPTION ""
        LANGUAGES CXX
)

# CMake Information
cmake_file_api(
        QUERY
        API_VERSION 1
        CODEMODEL 2.3
)

#-----------------------------------------------------------------------------------------------------------------------
# Settings
#-----------------------------------------------------------------------------------------------------------------------
if(PROJECT_IS_TOP_LEVEL)
    # C++ Settings
    set(CMAKE_CXX_STANDARD 26)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    set(CMAKE_CXX_EXTENSIONS OFF)
    set(CMAKE_CXX_VISIBILITY_PRESET hidden)
    set(CMAKE_VISIBILITY_INLINES_HIDDEN ON)
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

    # Project Settings
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

    # Project Options
    option(BUILD_TESTS "Build the project's test suite" OFF)

    # Set default build type if not specified
    if(NOT DEFINED CMAKE_BUILD_TYPE AND NOT DEFINED CMAKE_CONFIGURATION_TYPES)
        set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
        set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
    endif()
endif()

#-----------------------------------------------------------------------------------------------------------------------
# Dependencies
#-----------------------------------------------------------------------------------------------------------------------
if(BUILD_TESTS)
    find_package(GTest REQUIRED)
endif()

include(GNUInstallDirs)

#-----------------------------------------------------------------------------------------------------------------------
# Installation
#-----------------------------------------------------------------------------------------------------------------------
if(NOT CMAKE_SKIP_INSTALL_RULES)
    # Install headers
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/
            TYPE INCLUDE)
endif()

#-----------------------------------------------------------------------------------------------------------------------
# Target: cpp_map_headers
# Alias: collections::cpp_map_headers
# Type: Interface Library
#-----------------------------------------------------------------------------------------------------------------------
# Target definition
add_library(cpp_map_headers INTERFACE)

# Target alias
add_library(collections::cpp_map_headers ALIAS cpp_map_headers)

# Target headers
target_include_directories(cpp_map_headers
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

#-----------------------------------------------------------------------------------------------------------------------
# Target: cpp_map
# Alias: collections::cpp_map
# Type: Interface Library
# Dependencies: collections::cpp_map_headers
#-----------------------------------------------------------------------------------------------------------------------
# Target definition
add_library(cpp_map INTERFACE)

# Target alias
add_library(collections::cpp_map ALIAS cpp_map)

# Target dependencies
target_link_libraries(cpp_map
    INTERFACE
        collections::cpp_map_headers
)

# Target properties
set_target_properties(cpp_map PROPERTIES
        OUTPUT_NAME cpp_map
        VERSION ${PROJECT_VERSION}
        SOVERSION ${PROJECT_VERSION_MAJOR}
        CXX_VISIBILITY_PRESET hidden
        VISIBILITY_INLINES_HIDDEN ON
)

if(NOT CMAKE_SKIP_INSTALL_RULES)
    # Install cpp_map
    install(TARGETS cpp_map
            RUNTIME COMPONENT cpp_map
            LIBRARY COMPONENT cpp_map
            NAMELINK_COMPONENT cpp_map
            ARCHIVE COMPONENT cpp_map
            INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
endif()

#-----------------------------------------------------------------------------------------------------------------------
# Subdirectories
#-----------------------------------------------------------------------------------------------------------------------
if(BUILD_TESTS)
    add_subdirectory(tests)
endif()