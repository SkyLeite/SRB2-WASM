# Assets Target Configuration ##

# if(${CMAKE_SYSTEM} MATCHES Linux) Asset installation isn't part of the Linux
# target return() endif()

set(SRB2_CONFIG_ASSET_DIRECTORY ${PROJECT_SOURCE_DIR}/assets)

if("${SRB2_CONFIG_ASSET_DIRECTORY}" STREQUAL "")
  message(
    WARNING
      "SRB2_CONFIG_ASSET_DIRECTORY is not set, so installation will not contain data files."
  )
  return()
endif()

get_filename_component(SRB2_ASSET_DIRECTORY_ABSOLUTE
                       "${SRB2_CONFIG_ASSET_DIRECTORY}" ABSOLUTE)

set(SRB2_ASSETS_DOCS "README.txt" "README-SDL.txt" "LICENSE.txt"
                     "LICENSE-3RD-PARTY.txt")
list(TRANSFORM SRB2_ASSETS_DOCS PREPEND "/")
list(TRANSFORM SRB2_ASSETS_DOCS PREPEND "${SRB2_ASSET_DIRECTORY_ABSOLUTE}")

# ##############################################################################
# POST-V2.2 NOTE: Do not forget to add patch.pk3 to the end of this list!
# ##############################################################################

set(SRB2_ASSETS_GAME "srb2.pk3" "player.dta" "zones.pk3" "patch.pk3"
                     "music.dta" "models.dat")
list(TRANSFORM SRB2_ASSETS_GAME PREPEND "/")
list(TRANSFORM SRB2_ASSETS_GAME PREPEND "${SRB2_ASSET_DIRECTORY_ABSOLUTE}")

set(SRB2_ASSETS ${SRB2_ASSET_DOCS} ${SRB2_ASSETS_GAME})

file(
  COPY "${SRB2_CONFIG_ASSET_DIRECTORY}/"
  DESTINATION out
  FILES_MATCHING
  PATTERN "*.pk3")
file(
  COPY "${SRB2_CONFIG_ASSET_DIRECTORY}/"
  DESTINATION out
  FILES_MATCHING
  PATTERN "*.dta")
file(
  COPY "${SRB2_CONFIG_ASSET_DIRECTORY}/"
  DESTINATION out
  FILES_MATCHING
  PATTERN "*.dat")
# file(COPY "${SRB2_CONFIG_ASSET_DIRECTORY}/models" DESTINATION out/models)

# Installation

# install(FILES ${SRB2_ASSETS_GAME} DESTINATION .) install(DIRECTORY
# "${SRB2_ASSET_DIRECTORY_ABSOLUTE}/models" DESTINATION .)
