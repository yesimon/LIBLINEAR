cmake_minimum_required(VERSION 2.8)

project(linear)

file(GLOB blas_files blas/*.c)
add_library(linear SHARED linear.cpp tron.cpp ${blas_files})

# Copy python scripts over
set(python_dir ${CMAKE_SOURCE_DIR}/python)
file(GLOB python_files ${python_dir}/*.py)

add_custom_target(copy_python)
get_target_property(linear_location linear LOCATION)
get_filename_component(linear_dir ${linear_location} PATH)
foreach(python_file ${python_files})
  add_custom_command(TARGET copy_python PRE_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy ${python_file} .)
endforeach()

add_dependencies(linear copy_python)
