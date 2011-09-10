cmake_minimum_required(VERSION 2.6)

include(../cmake_globals.txt)

include_directories(
  ../
  ../engine/include
  ../engine_graphics/include
  src
)

set(OS_SOURCES "")

#definitions
add_definitions(
 -DVSX_FONT_NO_FT
 -DDEBUG
 -DVSX_TEXTURE_NO_RT
 -DVSXU_EXE
 -DVSXU_DEVELOPER
 -DVSXU_ARTISTE
 -DCMAKE_INSTALL_PREFIX="${CMAKE_INSTALL_PREFIX}"
)


if (VSXU_DEBUG EQUAL 1)
  add_definitions( -DVSXU_DEBUG )
endif (VSXU_DEBUG EQUAL 1)

get_filename_component(list_file_path ${CMAKE_CURRENT_LIST_FILE} PATH)
string(REGEX MATCH "[a-z._-]*$" module_id ${list_file_path})
message("*******************************************")
message("building            " ${module_id})
message("*******************************************")

set(SOURCES
  src/application.cpp
  src/main/linux/main.cpp
  )

link_directories(
./
)

project (${module_id})

add_executable(${module_id}  ${SOURCES} ${OS_SOURCES} ${EXTRA_LIBS})
include(../cmake_suffix.txt)
  set_target_properties(
    ${module_id}
      PROPERTIES
        OUTPUT_NAME
          vsxu_player_demo
  )

if(UNIX)
  link_directories("/usr/lib/vsxu")
  target_link_libraries(${module_id}
    Xrandr
    X11
    glfw-2.7
    pthread
    GLU
    GL
    GLEW.a
    png.a
    jpeg.a
    vsxu_engine_graphics
    vsxu_engine
  )
  install(TARGETS ${module_id} DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
  install(DIRECTORY ../share/visuals_faders ../share/visuals_player ../share/font DESTINATION ${CMAKE_INSTALL_PREFIX}/share/vsxu)
endif(UNIX)


if(WIN32)
  link_directories("../_lib/win32")
  
  target_link_libraries(${module_id}
  wsock32
  ws2_32
  glew
  pthreadGCE
  vsxu_engine_graphics
  vsxu_engine
  gdi32
  glu32
  opengl32
  )
endif(WIN32)

