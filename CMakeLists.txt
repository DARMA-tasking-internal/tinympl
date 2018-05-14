
cmake_minimum_required(VERSION 3.4)

project (TinyMPL
  VERSION 2.0
  LANGUAGES CXX
)

set(CMAKE_MODULE_PATH
  ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules
)


#------------------------------------------------------------------------------#
# tinympl interface library                                                    #
#------------------------------------------------------------------------------#

add_library(tinympl INTERFACE)
add_library(tinympl::tinympl ALIAS tinympl)

target_include_directories(tinympl INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/source/include>
  $<INSTALL_INTERFACE:include>
)
target_compile_features(tinympl INTERFACE cxx_std_14)

install(TARGETS tinympl EXPORT tinyMPLTargets
  INCLUDES DESTINATION include
)

install(EXPORT tinyMPLTargets
  FILE tinyMPLTargets.cmake
  NAMESPACE tinympl::
  DESTINATION cmake
)

export(TARGETS tinympl
  NAMESPACE tinympl::
  FILE tinyMPLTargets.cmake
)

install(DIRECTORY source/include/tinympl DESTINATION include FILES_MATCHING PATTERN "*.hpp")

################################################################################

include(CMakePackageConfigHelpers)
configure_package_config_file(cmake/TinyMPLConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/TinyMPLConfig.cmake
  INSTALL_DESTINATION cmake
)
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/TinyMPLConfigVersion.cmake
  COMPATIBILITY SameMajorVersion
)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/TinyMPLConfig.cmake ${CMAKE_CURRENT_BINARY_DIR}/TinyMPLConfigVersion.cmake
    DESTINATION cmake
)
