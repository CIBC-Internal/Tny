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

# Create static library to use.
add_library(cibc_tny ${Sources})