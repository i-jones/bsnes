add_library(gb 
    Core/apu.c
    Core/camera.c
    Core/debugger.c
    Core/rumble.c
    Core/display.c
    Core/gb.c
    Core/joypad.c
    Core/mbc.c
    Core/memory.c
    Core/printer.c
    Core/random.c
    Core/rewind.c
    Core/save_state.c
    Core/sgb.c
    Core/sm83_cpu.c
    Core/sm83_disassembler.c
    Core/symbol_hash.c
    Core/timing.c
)

target_compile_definitions(gb PRIVATE GB_INTERNAL)
target_compile_definitions(gb PRIVATE GB_DISABLE_CHEATS)
target_compile_definitions(gb PRIVATE _GNU_SOURCE)
target_compile_definitions(gb PRIVATE GB_VERSION="0.14.7")
target_compile_options(gb PRIVATE -Wno-multichar)