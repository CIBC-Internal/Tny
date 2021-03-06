#------------------------------------------------------------------------------
# TNY
#------------------------------------------------------------------------------

cmake_minimum_required(VERSION 2.8.7)

# Create a tny.hpp file that simply wraps the tny.h file in extern C.
set(TNY_CPP_HEADER_FILE "src/tny/tny.hpp")

if (NOT EXISTS "${TNY_CPP_HEADER_FILE}")
  file(WRITE ${TNY_CPP_HEADER_FILE} "#if defined __cplusplus\n")
  file(APPEND ${TNY_CPP_HEADER_FILE} "extern \"C\" {\n")
  file(APPEND ${TNY_CPP_HEADER_FILE} "#endif\n\n")
  file(APPEND ${TNY_CPP_HEADER_FILE} "#include \"tny.h\"\n\n")
  file(APPEND ${TNY_CPP_HEADER_FILE} "#if defined __cplusplus\n")
  file(APPEND ${TNY_CPP_HEADER_FILE} "};\n")
  file(APPEND ${TNY_CPP_HEADER_FILE} "#endif\n")
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_CFLAGS} -std=c99")

set (Sources
  src/tny/tny.c
  src/tny/tny.h)

SET(SCI_TNY_INCLUDE "${CMAKE_CURRENT_SOURCE_DIR}/src" CACHE INTERNAL "Tny include directories.")

SET(SCI_TNY_LIBRARY cibc_tny CACHE INTERNAL "tny library name.")
MESSAGE("cibc-tny: " ${SCI_TNY_LIBRARY})
# Create static library to use.
add_library(${SCI_TNY_LIBRARY} STATIC ${Sources})

set(SCI_TNY_LIBRARY_DIR ${CMAKE_CURRENT_BINARY_DIR})
set(TNY_USE_FILE "${CMAKE_CURRENT_SOURCE_DIR}/UseTny.cmake")

configure_file(TnyConfig.cmake.in "${CMAKE_CURRENT_BINARY_DIR}/TnyConfig.cmake" @ONLY)
export(TARGETS ${SCI_TNY_LIBRARY} FILE TnyExports.cmake)
