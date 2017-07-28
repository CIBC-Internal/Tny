#------------------------------------------------------------------------------
# TNY
#------------------------------------------------------------------------------

# Create a tny.hpp file that simply wraps the tny.h file in extern C.
set(TNY_CPP_HEADER_FILE "${SOURCE_DIR}/src/tny/tny.hpp")
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

file (GLOB Sources
  "${SOURCE_DIR}/src/tny/*.c"
  "${SOURCE_DIR}/src/tny/*.h"
  "${TNY_CPP_HEADER_FILE}")

# Create static library to use.
add_library(cibc_tny ${Sources})

