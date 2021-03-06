
cmake_minimum_required(VERSION 3.13 FATAL_ERROR)
project(vroom LANGUAGES CXX C)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

include(FindPkgConfig)
include(GNUInstallDirs)

set(CMAKE_CXX_STANDARD 17 CACHE STRING "C++ language version to use (default is 17)")
set(CMAKE_CXX_STANDARD_REQUIRED True)

option(USE_LIBOSRM "Enable libosrm support" OFF)
option(USE_LIBGLPK "Enable libglpk support" OFF)
option(BUILD_SHARED_LIBS "Build using shared libraries" OFF)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  message(STATUS "Configuring in debug mode")
elseif(CMAKE_BUILD_TYPE MATCHES Release)
  message(STATUS "Configuring in release mode")
elseif(CMAKE_BUILD_TYPE MATCHES RelWithDebInfo)
  message(STATUS "Configuring in release mode with debug flags")
elseif(CMAKE_BUILD_TYPE MATCHES MinRelSize)
  message(STATUS "Configuring in release mode with minimized size")
else()
  message(STATUS "Unrecognized build type - will use cmake defaults")
endif()

# Dependencies 
find_package(Threads REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(Asio REQUIRED)

# main targets, sources are populated in subdirs
add_executable(vroom "")

add_subdirectory(${CMAKE_SOURCE_DIR}/src)

target_link_libraries(vroom PRIVATE OpenSSL::Crypto)
target_link_libraries(vroom PRIVATE OpenSSL::SSL)
target_link_libraries(vroom PRIVATE Threads::Threads)


if(WIN32) 

  #disable iterator base class deprecation warning
  add_compile_definitions(_SILENCE_CXX17_ITERATOR_BASE_CLASS_DEPRECATION_WARNING)

  #additional library dependency  
  target_link_libraries(vroom PRIVATE Crypt32.lib)

  # prevent std::numeric_limits::max and other issues
  target_compile_definitions(vroom
    PUBLIC
      $<$<BOOL:${MSVC}>:NOMINMAX>
  )
endif()

target_include_directories(vroom PUBLIC "${CMAKE_SOURCE_DIR}/include")
target_include_directories(vroom PUBLIC "${CMAKE_SOURCE_DIR}/src")
target_include_directories(vroom PUBLIC "${ASIO_INCLUDE_DIR}")
