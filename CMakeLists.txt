cmake_minimum_required(VERSION 3.20)

project(my_project)

# Set the build type (Release by default)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build: Debug or Release" FORCE)
endif()

# Set C++ flags based on build type
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -Wall")
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
  if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -march=native")
  elseif(CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -march=native")
  elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /O2")
  endif()
endif()

# Find all source files
file(GLOB_RECURSE SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

# Find all header files
file(GLOB_RECURSE HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h" "${CMAKE_CURRENT_SOURCE_DIR}/include/*.hpp")

# Set include directories
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/include")

# Generate executable
add_executable(my_executable ${SOURCES} ${HEADERS})

# Generate static library
add_library(my_static_library STATIC ${SOURCES} ${HEADERS})

# Generate dynamic library
add_library(my_dynamic_library SHARED ${SOURCES} ${HEADERS})
set_target_properties(my_dynamic_library PROPERTIES OUTPUT_NAME "my_dynamic_library" PREFIX "")

# Let CMake detect compilers
enable_language(C)
enable_language(CXX)

# Generate Makefile
set(CMAKE_MAKE_PROGRAM "make")
set(CMAKE_C_COMPILER_ARG1 "")
set(CMAKE_CXX_COMPILER_ARG1 "")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(CMAKE_GENERATOR STREQUAL "Unix Makefiles")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror")
endif()
