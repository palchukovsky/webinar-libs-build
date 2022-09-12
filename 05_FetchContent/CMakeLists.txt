cmake_minimum_required(VERSION 3.15)

add_executable(fetch_content
  main.cpp)



include(FetchContent)

FetchContent_Declare(json
  GIT_REPOSITORY https://github.com/nlohmann/json.git
  GIT_TAG v3.10.4)

FetchContent_GetProperties(json)
if(NOT json_POPULATED)
  FetchContent_Populate(json)
  add_subdirectory(${json_SOURCE_DIR} ${json_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()

target_link_libraries(fetch_content
  PRIVATE nlohmann_json::nlohmann_json)



################################################################################

target_compile_features(fetch_content PUBLIC cxx_std_17)
set_target_properties(fetch_content PROPERTIES CXX_STANDARD_REQUIRED ON)

if(CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR
  CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(fetch_content
    PRIVATE
      -g
      -Wall
      -Wextra
      -Werror
      -pedantic)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  target_compile_options(fetch_content
    PRIVATE
      /EHsc
      /W4
      /WX)
endif()