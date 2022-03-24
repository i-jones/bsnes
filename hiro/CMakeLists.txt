file(GLOB_RECURSE hiro_headers CONFIGURE_DEPENDS *.hpp)

add_library(hiro hiro.cpp ${hiro_headers})

target_link_libraries(
  hiro
  PRIVATE 
        project_options
        project_warnings
  PUBLIC
        nall
)

if (APPLE)
        target_compile_definitions(hiro PRIVATE HIRO_COCOA)
        target_compile_options(hiro PRIVATE -x objective-c++)
        target_link_libraries(
                hiro
                PUBLIC
                        "-framework Cocoa"
                        "-framework Carbon"
                        "-framework IOKit"
                        "-framework Security"
        )
elseif(UNIX)
        target_compile_definitions(hiro PRIVATE HIRO_GTK=2)
        find_package(GTK2 2.8 REQUIRED gtk)
        target_include_directories(hiro PRIVATE ${GTK2_INCLUDE_DIRS})
        target_link_libraries(hiro PRIVATE ${GTK2_LIBRARIES})
elseif(WIN32)
# 
#hiro.flags   = $(flags.cpp) -DHIRO_WINDOWS
#hiro.options = -lkernel32 -luser32 -lgdi32 -ladvapi32 -lole32 -lcomctl32 -lcomdlg32 -luxtheme -lmsimg32 -lshlwapi -ldwmapi
        target_compile_definitions(hiro PRIVATE HIRO_WINDOWS)
        
endif()

          

