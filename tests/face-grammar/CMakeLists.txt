cmake_minimum_required(VERSION 3.11.4)
  
project(face-grammar)

add_subdirectory(extern/owl-cmake)

owl_generate_header(face.owl)

add_executable(run run.c face.owl.h)
