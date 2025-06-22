cmake_minimum_required(VERSION 4.0.0)

set(CMAKE_CXX_COMPILER /opt/homebrew/opt/llvm/bin/clang++) 
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_BUILD_TYPE Debug)

project(Jess VERSION 1.0.0 LANGUAGES CXX)

file(GLOB_RECURSE SOURCES "${PROJECT_SOURCE_DIR}/src/*")
add_executable(clang-test ${SOURCES})
target_include_directories(clang-test PRIVATE ${PROJECT_SOURCE_DIR})

if(APPLE) # llvm clang does not auto-discover system headers
  execute_process(
    COMMAND xcrun --show-sdk-path
    OUTPUT_VARIABLE MACOS_SDK_PATH
    OUTPUT_STRIP_TRAILING_WHITESPACE
  )
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -isysroot ${MACOS_SDK_PATH}")
endif()

