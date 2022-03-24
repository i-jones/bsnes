
file(GLOB_RECURSE ruby_headers CONFIGURE_DEPENDS *.hpp)

add_library(ruby ruby.cpp ${ruby_headers})


target_include_directories(ruby SYSTEM PUBLIC 
    "${CMAKE_CURRENT_SOURCE_DIR}/..")

target_link_libraries(
  ruby
  PRIVATE project_options
          project_warnings
)
if (APPLE)
        target_compile_definitions(ruby PRIVATE VIDEO_CGL)
        target_compile_definitions(ruby PRIVATE AUDIO_OPENAL)
        target_compile_definitions(ruby PRIVATE INPUT_QUARTZ)
        target_compile_options(ruby PRIVATE -x objective-c++)
        target_link_libraries(
                ruby
                PUBLIC
                        nall
                        "-framework OpenGL"
                        "-framework IOKit"
                        "-framework OpenAL")
elseif(UNIX)
        target_compile_definitions(
                ruby PRIVATE 
                VIDEO_GLX
                VIDEO_GLX2
                VIDEO_XVIDEO
                VIDEO_XSHM)
        target_compile_definitions(
                ruby PRIVATE 
                AUDIO_OSS
                AUDIO_ALSA)
        target_compile_definitions(ruby PRIVATE INPUT_XLIB)
        find_package(X11 REQUIRED)
        target_link_libraries(ruby PUBLIC X11::X11 X11::Xext X11::Xrandr X11::Xv)
        find_package(ALSA REQUIRED)
        target_link_libraries(ruby PUBLIC ALSA::ALSA)
        find_package(OpenGL REQUIRED COMPONENTS OpenGL GLX)
        target_link_libraries(ruby PUBLIC OpenGL::OpenGL OpenGL::GLX)
elseif(WIN32)
        target_compile_definitions(ruby PRIVATE
                VIDEO_WGL
                VIDEO_DIRECT3D
                VIDEO_DIRECTDRAW
                VIDEO_GDI)
        target_compile_definitions(ruby PRIVATE
                AUDIO_ASIO
                AUDIO_WASAPI
                AUDIO_XAUDIO2
                AUDIO_DIRECTSOUND
                AUDIO_WAVEOUT)
        target_compile_definitions(ruby PRIVATE
                INPUT_WINDOWS)
        target_link_libraries(ruby PUBLIC
                "opengl32.lib" 
                "d3d9.lib" 
                "ddraw.lib" 
                "avrt.lib"
                "uuid.lib"
                "ole32.lib"
                "dsound.lib"
                "winmm.lib"
                "dinput8.lib"
                "dxguid.lib")

endif()