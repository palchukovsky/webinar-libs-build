cmake_minimum_required(VERSION 3.15)

add_executable(linking
  main.cpp)



# Adding directory with the required library
# by directive "target_include_directories", the same as in 1st example.
#
target_include_directories(linking
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/googletest/googletest/include)

# But if the library has to be built - it's not enough to show where
# its header files are. It also requires showing where binary files are.
#
# Adding directory with the required library binaries
# by directive "target_link_directories".
# 
# Specifying with which libraries, the project has to be linked by the detective.
#
# You can check such arguments by CMake argument "VERBOSE", ex:
#   cmake --build build -- VERBOSE=1 
#
target_link_directories(linking
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/googletest/build/lib)

target_link_libraries(linking
  PUBLIC gtest gtest_main)



################################################################################

target_compile_features(linking PUBLIC cxx_std_17)
set_target_properties(linking PROPERTIES CXX_STANDARD_REQUIRED ON)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
  CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(linking
    PRIVATE
      -g
      -Wall
      -Wextra
      -Werror
      -pedantic)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_compile_options(linking
    PRIVATE
      /EHsc
      /W4
      /WX)
endif()