#Get a list of all of the files in the data-folder
file(GLOB_RECURSE SHADER_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/../../shaders/*")

file(GLOB_RECURSE DATABASE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/../Database/*")

set(bsnes_sources
  bsnes.cpp
  program/program.cpp
  input/input.cpp
  presentation/presentation.cpp
  settings/settings.cpp
  tools/tools.cpp
  resource/resource.cpp
  ${SHADER_SOURCES}
  ${DATABASE_SOURCES}
)

if (APPLE)
  set(bsnes_sources ${bsnes_sources} ${CMAKE_CURRENT_BINARY_DIR}/bsnes.icns)
  add_custom_target(icon_file
  DEPENDS
      "${CMAKE_CURRENT_BINARY_DIR}/bsnes.icns"
  )

  add_custom_command(
  OUTPUT
      "${CMAKE_CURRENT_BINARY_DIR}/bsnes.icns"
  COMMAND
      sips -s format icns ${CMAKE_CURRENT_SOURCE_DIR}/resource/bsnes.png --out  ${CMAKE_CURRENT_BINARY_DIR}/bsnes.icns
  ) 
  add_executable(bsnes MACOSX_BUNDLE
      ${bsnes_sources}
  )

  #individually set the file's path properties
  foreach(SHADER_FILE ${SHADER_SOURCES})

    #Get the relative path from the data-folder to the particular file
    file(RELATIVE_PATH SHADER_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../shaders" ${SHADER_FILE})
    get_filename_component(SHADER_PATH_DIR ${SHADER_PATH}  DIRECTORY)

    #Set it's location inside the app package (under Resources)
    set_property(SOURCE ${SHADER_FILE} PROPERTY MACOSX_PACKAGE_LOCATION "MacOS/Shaders/${SHADER_PATH_DIR}")
    #message("File ${SHADER_FILE} MacOS/Shaders/${SHADER_PATH_DIR}")

  endforeach(SHADER_FILE)

  #individually set the file's path properties
  foreach(DATABASE_FILE ${DATABASE_SOURCES})

    #Get the relative path from the data-folder to the particular file
    file(RELATIVE_PATH DATABASE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../Database" ${DATABASE_FILE})

    #Set it's location inside the app package (under Resources)
    set_property(SOURCE ${DATABASE_FILE} PROPERTY MACOSX_PACKAGE_LOCATION "MacOS/Database/")
    #message("File ${DATABASE_FILE} MacOS/Database/${DATABASE_PATH}")

  endforeach(DATABASE_FILE)

  set_property(SOURCE ${CMAKE_CURRENT_BINARY_DIR}/bsnes.icns PROPERTY MACOSX_PACKAGE_LOCATION "Resources")
else()
  add_executable(bsnes
    ${bsnes_sources}
  )
endif()

target_include_directories(bsnes SYSTEM PRIVATE .)
target_link_libraries(
  bsnes
  PRIVATE
    project_options
    project_warnings
    libbsnes
    ruby
    hiro
    gb
)

if (APPLE)
  set_target_properties(bsnes PROPERTIES
            BUNDLE True
            MACOSX_BUNDLE_GUI_IDENTIFIER my.domain.style.identifier.bsnes
            MACOSX_BUNDLE_BUNDLE_NAME bsnes
            MACOSX_BUNDLE_BUNDLE_VERSION "0.1"
            MACOSX_BUNDLE_SHORT_VERSION_STRING "0.1"
            MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/resource/bsnes.plist"
        )

  add_dependencies(bsnes icon_file)
endif()