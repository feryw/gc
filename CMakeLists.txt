#
# Copyright (C) 2021 HandsomeMod Project
#
# GC (Gadget Controller) is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation; either
# version 2.1 of the License, or (at your option) any later version.
#
cmake_minimum_required(VERSION 3.10)
project(GC C)

set(CMAKE_C_STANDARD 11)#C11
set(CMAKE_CXX_STANDARD 17)#C17
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_INSTALL_PREFIX /usr)
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR})
#file(GLOB_RECURSE INCLUDES  "gc_*.h")
file(GLOB_RECURSE SOURCES  "main.c" "gc_*.c")
SET(EXECUTABLE_OUTPUT_PATH  ${PROJECT_SOURCE_DIR}/bin)

# Core Dependency: link static libusbgx
# Adjust path if libusbgx.a is installed elsewhere
set(LIBUSBGX_STATIC "/usr/local/lib/libusbgx.a")
set(LIBUSBGX_INCLUDE_DIRS "/usr/local/include")

include_directories(${LIBUSBGX_INCLUDE_DIRS})

# generated headers
configure_file(
    "gc_config.h.in"
    "${PROJECT_SOURCE_DIR}/gc_config.h"
)

add_executable(gc ${SOURCES})
target_link_libraries(gc PRIVATE ${LIBUSBGX_STATIC})

# Force fully static binary (glibc + libgcc + libusbgx)
set_target_properties(gc PROPERTIES LINK_FLAGS "-static -static-libgcc -static-libstdc++")

install(FILES ${PROJECT_SOURCE_DIR}/bin/gc DESTINATION bin)


