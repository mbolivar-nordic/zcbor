#
# Copyright (c) 2021 Nordic Semiconductor ASA
#
# SPDX-License-Identifier: Apache-2.0
#

cmake_minimum_required(VERSION 3.13.1)

project(test8_suit12)
include(../../cmake/test_template.cmake)

set(py_command
  zcbor
  -c ${CMAKE_CURRENT_LIST_DIR}/../../cases/manifest12.cddl
  code
  --output-cmake ${PROJECT_BINARY_DIR}/manifest12.cmake
  -t
  SUIT_Envelope_Tagged
  SUIT_Command_Sequence
  SUIT_Envelope
  -d
  ${bit_arg}
  )

execute_process(
  COMMAND
  ${py_command}
  COMMAND_ERROR_IS_FATAL ANY
  )

target_sources(app PRIVATE
  ../../fuzz/fuzz_manifest12.c
  )

include(${PROJECT_BINARY_DIR}/manifest12.cmake)

target_link_libraries(manifest12 PRIVATE zephyr_interface)
target_link_libraries(app PRIVATE manifest12)
