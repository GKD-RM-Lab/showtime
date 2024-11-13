cmake_minimum_required(VERSION 3.30)

project(
    showtime
    VERSION 0.1
    DESCRIPTION "Robot Controller for Robomaster"
    LANGUAGES C CXX)

set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Og")
set(CMAKE_BUILD_PARALLEL_LEVEL ${CMAKE_CPU_COUNT})

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake;${CMAKE_MODULE_PATH}")

file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/ext")

include(tomlplusplus)
include(imgui)
include(implot)

file(GLOB SRC CONFIGURE_DEPENDS src/*.cc)

add_library(${PROJECT_NAME} SHARED ${SRC})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} src)
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_23)
target_compile_options(${PROJECT_NAME} PRIVATE -Wall)
target_link_libraries(${PROJECT_NAME} PUBLIC imgui implot tomlplusplus)

