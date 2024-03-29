cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(print)

add_library(${PROJECT_NAME} STATIC source/print.cpp)

target_include_directories(${PROJECT_NAME} PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
        "$<INSTALL_INTERFACE:include>")

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

install(TARGETS print
	EXPORT "${TARGETS_EXPORT_NAME}"
	RUNTIME DESTINATION "bin"
	INCLUDES DESTINATION "include")
install(
    FILES "include/print.h"
    DESTINATION "include"
)
install(EXPORT "${TARGETS_EXPORT_NAME}"
  	FILE "${TARGETS_EXPORT_NAME}.cmake"
  	DESTINATION "${config_bin}")
install(FILES "${project_config}"
	DESTINATION "${config_bin}")

