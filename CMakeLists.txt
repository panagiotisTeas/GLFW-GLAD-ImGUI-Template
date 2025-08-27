cmake_minimum_required(VERSION 3.22)

project(GraphicsTools
    LANGUAGES C CXX)

add_subdirectory(dependencies/)

# Executable

add_executable(test)

# Sources

target_sources(test PUBLIC
    test.cpp)

# Libraries

target_link_libraries(test
    PUBLIC glad
    PUBLIC glfw
    PUBLIC imgui)