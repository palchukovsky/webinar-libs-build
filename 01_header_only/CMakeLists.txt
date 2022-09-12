cmake_minimum_required(VERSION 3.15)

add_executable(header_only
  main.cpp)



# Adding directory with the required library
# by directive "target_include_directories".
#
# This directive adds a specific argument for the compiler (argument depends
# on platform) to show the path where the compiler could search
# for header files (to include it).
#
# You can check such arguments by CMake argument "VERBOSE", ex:
#   cmake --build build -- VERBOSE=1 
#
target_include_directories(header_only
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/json/include)



################################################################################

target_compile_features(header_only PUBLIC cxx_std_17)
set_target_properties(header_only PROPERTIES CXX_STANDARD_REQUIRED ON)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
  CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(header_only
    PRIVATE
      -g
      -Wall
      -Wextra
      -Werror
      -pedantic)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_compile_options(header_only
    PRIVATE
      /EHsc
      /W4
      /WX)
endif()