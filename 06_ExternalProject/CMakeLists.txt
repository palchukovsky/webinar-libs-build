cmake_minimum_required(VERSION 3.15)

add_executable(external_project
  main.cpp)



include(ExternalProject)

ExternalProject_Add(openssl
  PREFIX openssl
  GIT_REPOSITORY https://github.com/openssl/openssl.git
  GIT_TAG openssl-3.0.5
  GIT_SHALLOW 1 # tell Git to clone with --depth 1 
  CONFIGURE_COMMAND ${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl/Configure --prefix=${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl-build --openssldir=${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl-build
  BUILD_COMMAND make ${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl -j 8
  INSTALL_COMMAND make ${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl install)

add_dependencies(external_project openssl)

target_link_directories(external_project
  PUBLIC ${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl-build/include/lib64)

target_include_directories(external_project
  PUBLIC ${CMAKE_BINARY_DIR}/06_ExternalProject/openssl/src/openssl-build/include)

target_link_libraries(external_project
  PUBLIC ssl crypto)



################################################################################

target_compile_features(external_project PUBLIC cxx_std_17)
set_target_properties(external_project PROPERTIES CXX_STANDARD_REQUIRED ON)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
  CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(external_project
    PRIVATE
      -g
      -Wall
      -Wextra
      -Werror
      -pedantic)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_compile_options(external_project
    PRIVATE
      /EHsc
      /W4
      /WX)
endif()