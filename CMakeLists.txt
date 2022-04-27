cmake_minimum_required(VERSION 3.22)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_foo)

add_library(${PROJECT_NAME} library.cpp)

set(config_bin "lib/cmake/${PROJECT_NAME}")
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(TARGETS_EXPORT_NAME "${PROJECT_NAME}")

include(CMakePackageConfigHelpers)

configure_package_config_file(
        "cmake/Config.cmake.in"
        "${project_config}"
        INSTALL_DESTINATION "${config_bin}"
)

install(
        TARGETS hello_foo
        EXPORT "${TARGETS_EXPORT_NAME}"
        RUNTIME DESTINATION "bin"
)
install(
        FILES "library.h"
        DESTINATION "."
)
install(
        EXPORT "${TARGETS_EXPORT_NAME}"
        FILE "${TARGETS_EXPORT_NAME}.cmake"
        DESTINATION "${config_bin}"
)
install(
        FILES "${project_config}"
        DESTINATION "${config_bin}"
)