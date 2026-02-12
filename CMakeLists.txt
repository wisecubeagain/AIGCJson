# AIGCJson - 类/结构体与 JSON 互转的头文件库（基于 rapidjson）
#
# 他人集成方式一（子目录）：
#   add_subdirectory(path/to/AIGCJson)
#   target_link_libraries(your_target PRIVATE AIGCJson)
#
# 他人集成方式二（仅头文件）：
#   target_include_directories(your_target PRIVATE path/to/AIGCJson/include)
#
# 关闭测试：cmake -DAIGCJSON_BUILD_TESTS=OFF ..
#
cmake_minimum_required(VERSION 3.10)
project(AIGCJson VERSION 1.0.2 LANGUAGES CXX
  DESCRIPTION "Header-only C++ library for class/struct and JSON conversion"
  HOMEPAGE_URL "https://github.com/AIGMix/AIGCJson")

# -----------------------------------------------------------------------------
# AIGCJson 头文件库（依赖 rapidjson，已内置于 include/rapidjson）
# -----------------------------------------------------------------------------
add_library(AIGCJson INTERFACE)
target_include_directories(AIGCJson INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_features(AIGCJson INTERFACE cxx_std_11)

# -----------------------------------------------------------------------------
# 安装：便于他人通过 find_package 或安装后 -I 使用
# -----------------------------------------------------------------------------
include(GNUInstallDirs)
install(DIRECTORY include/
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp")
install(TARGETS AIGCJson
  EXPORT AIGCJsonTargets
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
include(CMakePackageConfigHelpers)
install(EXPORT AIGCJsonTargets
  FILE AIGCJsonTargets.cmake
  NAMESPACE AIGCJson::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/AIGCJson)

# -----------------------------------------------------------------------------
# 测试（可选）
# -----------------------------------------------------------------------------
option(AIGCJSON_BUILD_TESTS "Build AIGCJson tests" ON)
if(AIGCJSON_BUILD_TESTS)
  enable_testing()
  add_subdirectory(test)
endif()
