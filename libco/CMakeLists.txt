add_library(libco libco.c)

target_link_libraries(
  libco
  PRIVATE project_options
          project_warnings)

target_include_directories(libco SYSTEM PUBLIC 
          "${CMAKE_CURRENT_SOURCE_DIR}/..")