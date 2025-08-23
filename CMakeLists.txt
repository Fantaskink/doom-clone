cmake_minimum_required(VERSION 3.10)

project(doom-clone C)

set(CMAKE_C_STANDARD 99)

# Compiler flags for warnings, color diagnostics, and debug info
add_compile_options(-Wall -Werror -g -O2 -fdiagnostics-color=always)

# Add include directories
include_directories(${CMAKE_SOURCE_DIR}/include)

# Add source files (explicitly add gl.c)
file(GLOB SOURCES ${CMAKE_SOURCE_DIR}/src/*.c)
list(FIND SOURCES "${CMAKE_SOURCE_DIR}/src/gl.c" GLAD_INDEX)
if(GLAD_INDEX EQUAL -1)
    list(APPEND SOURCES ${CMAKE_SOURCE_DIR}/src/gl.c)
endif()

# Add executable
add_executable(doom-clone ${SOURCES})

# Link GLFW (dylib)
target_link_libraries(doom-clone PRIVATE ${CMAKE_SOURCE_DIR}/lib/libglfw.3.4.dylib)

# Link OpenGL framework on macOS
if(APPLE)
    target_link_libraries(doom-clone PRIVATE "-framework OpenGL")
endif()

# Set rpath for local lib folder (macOS)
set_target_properties(doom-clone PROPERTIES INSTALL_RPATH "@loader_path/../lib")
