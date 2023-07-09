# Declare SDL2 interface sources

if("${CMAKE_BUILD_TYPE}" EQUAL "Release")
  add_compile_options("SHELL: -O3")
endif()

target_sources(SRB2SDL2 PRIVATE mixer_sound.c)

target_sourcefile(c)

target_sources(SRB2SDL2 PRIVATE ogl_sdl.c)

target_sources(SRB2SDL2 PRIVATE i_threads.c)

if("${CMAKE_SYSTEM_NAME}" MATCHES Linux)
  target_link_libraries(SRB2SDL2 PRIVATE m rt)
endif()

add_link_options("SHELL: -lidbfs.js")
add_link_options("SHELL: -s EXPORTED_RUNTIME_METHODS=ccall,cwrap")
add_link_options(
  "SHELL: -s EXPORTED_FUNCTIONS=_main,_Command_ListWADS_f,_COM_ExecuteString,_P_AddWadFile"
)
add_link_options("SHELL: -s ASYNCIFY")
add_link_options("SHELL: -sUSE_SDL=2")
add_link_options("SHELL: -sUSE_ZLIB=1")
add_link_options("SHELL: -sUSE_SDL_MIXER=2")
add_link_options("SHELL: -s ALLOW_MEMORY_GROWTH=1")
add_link_options("SHELL: -s INITIAL_MEMORY=655360000")
add_link_options("SHELL: -s STACK_SIZE=65536000")
add_link_options("--preload-file=${CMAKE_BINARY_DIR}/assets/out@/")
add_link_options("SHELL: --use-preload-plugins")

target_compile_definitions(SRB2SDL2 PRIVATE -DHAVE_MIXER -DSOUND=SOUND_MIXER)
target_compile_definitions(
  SRB2SDL2 PRIVATE -DDIRECTFULLSCREEN -DHAVE_SDL -DNEWSIGNALHANDLER=0
                   -DUNIXBACKTRACE=0 -DHAVE_PNG)
target_link_options(
  SRB2SDL2
  PRIVATE
  -sUSE_SDL=2
  -sUSE_SDL_MIXER=2
  -sUSE_ZLIB
  -sUSE_LIBPNG
  -lidbfs.js
  -sEXPORTED_RUNTIME_METHODS=ccall,cwrap
  -sEXPORTED_FUNCTIONS=_main,_P_AddWadFile
  -sASYNCIFY
  -sALLOW_MEMORY_GROWTH=1
  -sINITIAL_MEMORY=983040000
  -sSTACK_SIZE=655360000
  --preload-file=${CMAKE_BINARY_DIR}/assets/out@/
  --use-preload-plugins)
target_compile_options(SRB2SDL2 PRIVATE -sUSE_SDL=2 -sUSE_SDL_MIXER=2
                                        -sUSE_ZLIB -sUSE_LIBPNG)

# Installation ####
install(TARGETS SRB2SDL2 SRB2SDL2 RUNTIME DESTINATION .)

set(SRB2_SDL2_AVAILABLE
    YES
    PARENT_SCOPE)
