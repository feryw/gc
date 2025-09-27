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

# generated headers
configure_file(
    "gc_config.h.in"
    "${PROJECT_SOURCE_DIR}/gc_config.h"
)

add_executable(gc ${SOURCES})
target_include_directories(gc PRIVATE /tmp/sysroot/include ${PROJECT_SOURCE_DIR})
target_link_libraries(gc PRIVATE
    /tmp/sysroot/lib/libusbgx.a
   /usr/lib/arm-linux-gnueabihf/libconfig.a
)

install(FILES ${PROJECT_SOURCE_DIR}/bin/gc DESTINATION bin)


