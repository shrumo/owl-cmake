cmake_minimum_required(VERSION 3.11)

project(owl-cmake)

option(OWL_USE_INCLUDED_MAKE "Whether to use the makefile provided with owl code to build owl." 0)
if(OWL_USE_INCLUDED_MAKE)
    # Use add_executable here and add_custom_command on target
    add_custom_target(owl COMMAND make WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/extern/owl
                     BYPRODUCTS ${PROJECT_SOURCE_DIR}/extern/owl/owl)
    set_target_properties(owl PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/extern/owl/)
else()
    file(GLOB OWL_SRCS "${PROJECT_SOURCE_DIR}/extern/owl/src/*.c" "${PROJECT_SOURCE_DIR}/extern/owl/src/*.h")
    add_executable(owl ${OWL_SRCS})
    target_link_libraries(owl dl)
endif()


# Processes grammar_file and build a header file.
# owl_generate_header(<SRC>)
# Example:
#   owl_generate_header(C.owl) # produces C.owl.h file in the same place the original C.owl resides
function(owl_generate_header grammar_file_path)
    add_custom_command(OUTPUT "${grammar_file_path}.h"
                       DEPENDS "${grammar_file_path}"
                       COMMAND owl -c ${grammar_file_path} -o ${grammar_file_path}.h
                       WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                       )
endfunction()
