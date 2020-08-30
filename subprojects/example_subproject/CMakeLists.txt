CMAKE_MINIMUM_REQUIRED (VERSION 3.0.0)
PROJECT (example_subproject LANGUAGES C)

FILE(GLOB main_src main.c)
FILE(GLOB main_h main.h)
SET(gen_dir ${CMAKE_CURRENT_BINARY_DIR}/somewhere/gen)
SET(main_h_gen ${gen_dir}/main.h)

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR}/somewhere)
ADD_CUSTOM_COMMAND(
  OUTPUT ${main_h_gen}
  COMMAND ${CMAKE_COMMAND}
  ARGS -E copy ${main_h} ${main_h_gen}
  COMMENT "Copying ${main_h} to ${main_h_gen}"
  VERBATIM)

ADD_LIBRARY(main STATIC ${main_src} ${main_h_gen})
