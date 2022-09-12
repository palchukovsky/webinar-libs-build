cmake_minimum_required(VERSION 3.15)

add_executable(sha256_from_HelloWorld
  main.cpp)



target_include_directories(sha256_from_HelloWorld
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/openssl/build/include)

target_link_directories(sha256_from_HelloWorld
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/openssl/build/lib)

target_link_libraries(sha256_from_HelloWorld
  PUBLIC ssl crypto)



################################################################################

target_compile_features(sha256_from_HelloWorld PUBLIC cxx_std_17)
set_target_properties(sha256_from_HelloWorld PROPERTIES CXX_STANDARD_REQUIRED ON)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
  CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(sha256_from_HelloWorld
    PRIVATE
      -g
      -Wall
      -Wextra
      -Werror
      -pedantic)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_compile_options(sha256_from_HelloWorld
    PRIVATE
      /EHsc
      /W4
      /WX)
endif()