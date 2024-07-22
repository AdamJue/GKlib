cmake_minimum_required(VERSION 3.22)
project(GKlib C)

option(BUILD_SHARED_LIBS "Build shared libraries (.dll/.so) instead of static ones (.lib/.a)" OFF)

get_filename_component(abs "." ABSOLUTE)
set(GKLIB_PATH ${abs})
unset(abs)
include(GKlibSystem.cmake)

include_directories(".")
if(MSVC)
  include_directories("win32")
  file(GLOB win32_sources RELATIVE "win32" "*.c")
else(MSVC)
  set(win32_sources, "")
endif(MSVC)

add_library(GKlib ${GKlib_sources} ${win32_sources})

if(UNIX)
  target_link_libraries(GKlib m)
endif(UNIX)

install(TARGETS GKlib
   EXPORT GKlibTargets
   INCLUDES DESTINATION "include/GKlib"
  )
install(EXPORT GKlibTargets FILE "GKlibConfig.cmake" DESTINATION "share/gklib")
install(FILES "win32/adapt.h" DESTINATION "include/${HINSTALL_PATH}/win32")  
install(FILES ${GKlib_includes} DESTINATION include/${HINSTALL_PATH})
