add_subdirectory(gb)
add_subdirectory(target-bsnes)
add_subdirectory(target-libretro)

file(GLOB_RECURSE emulator_headers CONFIGURE_DEPENDS emulator/*.hpp)
file(GLOB_RECURSE filter_headers CONFIGURE_DEPENDS filter/*.hpp)
file(GLOB_RECURSE heuristics_headers CONFIGURE_DEPENDS heuristics/*.hpp)
file(GLOB_RECURSE lzma_headers CONFIGURE_DEPENDS lzma/*.hpp)
file(GLOB_RECURSE processor_headers CONFIGURE_DEPENDS processor/*.hpp)
file(GLOB_RECURSE sfc_headers CONFIGURE_DEPENDS sfc/*.hpp)

add_library(libbsnes 
    emulator/emulator.cpp
    filter/filter.cpp
    lzma/lzma.cpp
    
    sfc/interface/interface.cpp
    sfc/system/system.cpp
    sfc/controller/controller.cpp
    sfc/cartridge/cartridge.cpp
    sfc/memory/memory.cpp

    sfc/cpu/cpu.cpp
    sfc/smp/smp.cpp
    sfc/dsp/dsp.cpp
    sfc/ppu/ppu.cpp
    sfc/ppu-fast/ppu.cpp

    sfc/expansion/expansion.cpp
    sfc/coprocessor/coprocessor.cpp
    sfc/slot/slot.cpp

    processor/arm7tdmi/arm7tdmi.cpp
    processor/spc700/spc700.cpp
    processor/wdc65816/wdc65816.cpp

    ${emulator_headers}
    ${filter_headers}
    ${heuristics_headers}
    ${lzma_headers}
    ${processor_headers}
    ${sfc_headers}
)

target_include_directories(libbsnes SYSTEM PUBLIC .)
target_link_libraries(
    libbsnes
  PRIVATE 
    project_options
    project_warnings
  PUBLIC
    libco
)