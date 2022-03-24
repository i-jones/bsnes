add_library(bsnes_libretro SHARED
 libretro.cpp
 libretro.h
 )
target_link_libraries(
    bsnes_libretro
    PRIVATE
        project_options
        project_warnings
        libbsnes
        gb
)

target_compile_options(
    bsnes_libretro 
    PRIVATE
        -Wno-narrowing 
        -Wno-multichar)
set_property(
    TARGET 
        bsnes_libretro
    PROPERTY
        POSITION_INDEPENDENT_CODE ON)
set_target_properties(bsnes_libretro PROPERTIES PREFIX "")
