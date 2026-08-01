#-----------------------------------------------------------------------------------------------------------------------
# Google Test
#-----------------------------------------------------------------------------------------------------------------------
include(GoogleTest)
enable_testing()

#-----------------------------------------------------------------------------------------------------------------------
# Target: cpp_map_test
# Alias: collections::cpp_map_test
# Type: Executable
# Dependencies: GTest::gtest_main, GTest::gmock_main, collections::cpp_map_headers
#-----------------------------------------------------------------------------------------------------------------------
# Target definition
add_executable(cpp_map_test)

# Target alias
add_executable(collections::cpp_map_test ALIAS cpp_map_test)

# Target sources
target_sources(cpp_map_test
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/cpp_map_test.cpp
)

# Target dependencies
target_link_libraries(cpp_map_test
    PRIVATE
        collections::cpp_map_headers
        GTest::gtest_main
        GTest::gmock_main
)

# Test discovery
gtest_discover_tests(cpp_map_test)