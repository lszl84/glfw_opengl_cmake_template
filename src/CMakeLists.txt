cmake_minimum_required(VERSION 3.6 FATAL_ERROR)

project(glfw_opengl_cmake_template_core LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

file(TO_NATIVE_PATH "${STAGED_INSTALL_PREFIX}/lib/cmake/glfw3" glfw3_DIR)
file(TO_NATIVE_PATH "${STAGED_INSTALL_PREFIX}/lib/cmake/glew" GLEW_DIR)

message(STATUS "glfw3_DIR is... ${glfw3_DIR}")
message(STATUS "GLEW_DIR is... ${GLEW_DIR}")

find_package(glfw3 REQUIRED CONFIG)

set(GLEW_USE_STATIC_LIBS ON)
find_package(GLEW REQUIRED CONFIG)

set(SRCS main.cpp)

if(APPLE)
    # create bundle on apple compiles
    add_executable(main MACOSX_BUNDLE ${SRCS})

    # Set a custom plist file for the app bundle - needed for Mac OS Retina display
    set_target_properties(main PROPERTIES MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/Info.plist)
else()
    # No WIN32! We don't want WinMain() with GLFW.
    add_executable(main ${SRCS} main.exe.manifest)
endif()

target_link_libraries(main PRIVATE GLEW::glew_s glfw)