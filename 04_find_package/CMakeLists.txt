cmake_minimum_required(VERSION 3.15)

add_executable(find_package
  main.cpp)



# We could ask CMake to find the required package by itself.
# Directive find_package uses path list from
# the argument -DCMAKE_PREFIX_PATH (where -D means "set define").
#
find_package(nlohmann_json 3.10.4 REQUIRED)
target_link_libraries(find_package
  PRIVATE nlohmann_json::nlohmann_json)


################################################################################

target_compile_features(find_package PUBLIC cxx_std_17)
set_target_properties(find_package PROPERTIES CXX_STANDARD_REQUIRED ON)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
  CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(find_package
    PRIVATE
      -g
      -Wall
      -Wextra
      -Werror
      -pedantic)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_compile_options(find_package
    PRIVATE
      /EHsc
      /W4
      /WX)
endif()