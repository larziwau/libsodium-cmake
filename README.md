# libsodium-cmake

NOTE: this is made specifically for use in Globed, not for general purpose use. I didn't want to spend time configuring stuff and I just deleted everything here that is not needed for my purpose.

This fork bumps the version to 1.0.19, disables tests and explicitly enables the static library build.

You should probably see the [original repository](https://github.com/robinlinden/libsodium-cmake) instead for any other purpose.

## Description

CMakeWrapper for [libsodium](https://github.com/jedisct1/libsodium), the modern, portable, easy to use crypto library.

This wrapper is written with a few goals in mind:
1. It should be easy to build
1. It should be obvious that libsodium's source code hasn't been touched
1. It should be easy to integrate into projects

## Building

Clone!

`git clone --recursive https://github.com/robinlinden/libsodium-cmake.git`

Build!

```sh
mkdir build && cd build
cmake ..
make
make test
```

## Using in your project

```cmake
cmake_minimum_required(VERSION 3.14)
# 3.11 and higher is supported, but this example uses
# `FetchContent_MakeAvailable` which is only available starting with 3.14.

include(FetchContent)

# Update the commit to point to whatever libsodium-cmake-commit you want to target.
FetchContent_Declare(Sodium
    GIT_REPOSITORY https://github.com/robinlinden/libsodium-cmake.git
    GIT_TAG 99f14233eab1d4f7f49c2af4ec836f2e701c445e # HEAD as of 2022-05-28
)
set(SODIUM_DISABLE_TESTS ON)
FetchContent_MakeAvailable(Sodium)

target_link_libraries(${PROJECT_NAME}
    PRIVATE
        sodium
)
```
