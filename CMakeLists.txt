cmake_minimum_required(VERSION 3.11)

if(POLICY CMP0077)
    cmake_policy(SET CMP0077 NEW)
endif()

project("sodium" LANGUAGES C)

set(BUILD_SHARED_LIBS OFF)
set(SODIUM_STATIC ON)
set(SODIUM_EXPORT)

option(SODIUM_ENABLE_BLOCKING_RANDOM "Enable this switch only if /dev/urandom is totally broken on the target platform" OFF)

file(GLOB_RECURSE SOURCES libsodium/src/*.c)

add_library(${PROJECT_NAME} STATIC ${SOURCES})

set_target_properties(${PROJECT_NAME}
    PROPERTIES
        C_STANDARD 99
)

target_include_directories(${PROJECT_NAME}
    PUBLIC
        libsodium/src/libsodium/include
    PRIVATE
        libsodium/src/libsodium/include/sodium
)

target_compile_definitions(${PROJECT_NAME}
    PUBLIC
        $<$<NOT:$<BOOL:${BUILD_SHARED_LIBS}>>:SODIUM_STATIC>
        $<$<BOOL:${SODIUM_MINIMAL}>:SODIUM_LIBRARY_MINIMAL>
    PRIVATE
        CONFIGURED
        $<$<BOOL:${BUILD_SHARED_LIBS}>:SODIUM_DLL_EXPORT>
        $<$<BOOL:${SODIUM_ENABLE_BLOCKING_RANDOM}>:USE_BLOCKING_RANDOM>
        $<$<BOOL:${SODIUM_MINIMAL}>:MINIMAL>
        $<$<C_COMPILER_ID:MSVC>:_CRT_SECURE_NO_WARNINGS>
)

# Variables that need to be exported to version.h.in
set(VERSION 1.0.19)
set(SODIUM_LIBRARY_VERSION_MAJOR 10)
set(SODIUM_LIBRARY_VERSION_MINOR 3)

configure_file(
    libsodium/src/libsodium/include/sodium/version.h.in
    ${CMAKE_CURRENT_SOURCE_DIR}/libsodium/src/libsodium/include/sodium/version.h
)