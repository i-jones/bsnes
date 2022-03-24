
file(GLOB_RECURSE nall_sources CONFIGURE_DEPENDS
     *.hpp)
add_library(nall INTERFACE
    ${nall_sources})
target_include_directories(nall SYSTEM INTERFACE 
    "${CMAKE_CURRENT_SOURCE_DIR}/..")
if (UNIX)
    target_link_libraries(nall INTERFACE ${CMAKE_DL_LIBS})
endif()