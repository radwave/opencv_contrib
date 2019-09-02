set(the_description "FreeType module. It enables to draw strings with outlines and mono-bitmaps/gray-bitmaps.")
if(APPLE_FRAMEWORK)
  ocv_module_disable(freetype)
endif()

FIND_PATH(FREETYPE_INCLUDE_DIRS
    NAMES freetype2/ft2build.h
    HINTS $ENV{FREETYPE_DIR}/include
        ${CMAKE_INSTALL_PREFIX}/include
    PATHS /usr/local/include
        /usr/include
    NO_CMAKE_FIND_ROOT_PATH
)
FIND_LIBRARY(
    FREETYPE_LIBRARIES
    NAMES freetype
    HINTS $ENV{FREETYPE_DIR}/lib
        ${CMAKE_INSTALL_PREFIX}/lib
        ${CMAKE_INSTALL_PREFIX}/lib64
    PATHS /usr/local/lib
        /usr/lib
        /usr/lib64
    NO_CMAKE_FIND_ROOT_PATH
)

if(FREETYPE_INCLUDE_DIRS AND FREETYPE_LIBRARIES)
    set(FREETYPE_FOUND TRUE)
endif()

FIND_PATH(HARFBUZZ_INCLUDE_DIRS
    NAMES harfbuzz/hb.h
    HINTS $ENV{HARFBUZZ_DIR}/include
        ${CMAKE_INSTALL_PREFIX}/include
    PATHS /usr/local/include
        /usr/include
    NO_CMAKE_FIND_ROOT_PATH
)
FIND_LIBRARY(
    HARFBUZZ_LIBRARIES
    NAMES harfbuzz
    HINTS $ENV{HARFBUZZ_DIR}/lib
        ${CMAKE_INSTALL_PREFIX}/lib
        ${CMAKE_INSTALL_PREFIX}/lib64
    PATHS /usr/local/lib
        /usr/lib
        /usr/lib64
    NO_CMAKE_FIND_ROOT_PATH
)
if(HARFBUZZ_INCLUDE_DIRS AND HARFBUZZ_LIBRARIES)
    set(HARFBUZZ_FOUND TRUE)
endif()

if(OPENCV_INITIAL_PASS)
  if(NOT FREETYPE_FOUND)
    message(STATUS "freetype2:   NO")
  else()
    message(STATUS "freetype2:   YES (ver ${FREETYPE_VERSION})")
  endif()

  if(NOT HARFBUZZ_FOUND)
    message(STATUS "harfbuzz:    NO")
  else()
    message(STATUS "harfbuzz:    YES (ver ${HARFBUZZ_VERSION})")
  endif()
endif()

if(FREETYPE_FOUND AND HARFBUZZ_FOUND)
  ocv_define_module(freetype opencv_core opencv_imgproc WRAP python)
  ocv_target_link_libraries(${the_module} ${FREETYPE_LIBRARIES} ${HARFBUZZ_LIBRARIES})
  ocv_include_directories( ${FREETYPE_INCLUDE_DIRS} ${HARFBUZZ_INCLUDE_DIRS} )
else()
  ocv_module_disable(freetype)
endif()
