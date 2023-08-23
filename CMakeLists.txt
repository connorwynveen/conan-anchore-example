cmake_minimum_required(VERSION 3.5)
project(HelloConan)

# This automatically sets up CMake to use Conan for dependencies.
include(${CMAKE_BINARY_DIR}/conan_paths.cmake)

add_executable(hello hello.c)
