#
# Copyright (c) 2008-2016 the Urho3D project.
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.
#

cmake_minimum_required(VERSION 3.0)

project(Detour VERSION 1.0.0)

set(TARGET_PUBLIC_HDRS
  include/DetourAlloc.h
  include/DetourAssert.h
  include/DetourCommon.h
  include/DetourMath.h
  include/DetourNavMesh.h
  include/DetourNavMeshBuilder.h
  include/DetourNavMeshQuery.h
  include/DetourNode.h
  include/DetourStatus.h
  )

set(TARGET_SOURCES
  source/DetourAlloc.cpp
  source/DetourCommon.cpp
  source/DetourNavMesh.cpp
  source/DetourNavMeshBuilder.cpp
  source/DetourNavMeshQuery.cpp
  source/DetourNode.cpp
  )

include_directories(include)
add_library(Detour ${TARGET_SOURCES})

#### install 

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

# Configuration
set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
  "${version_config}" COMPATIBILITY SameMajorVersion
  )

configure_package_config_file(
  "cmake/Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
  )

install(
  TARGETS Detour
  EXPORT "${targets_export_name}"
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin"
  INCLUDES DESTINATION "${include_install_dir}"
  )

foreach(HEADER ${TARGET_PUBLIC_HDRS})
  install(
    FILES "${CMAKE_CURRENT_SOURCE_DIR}/${HEADER}"
    DESTINATION "${include_install_dir}/${PROJECT_NAME}"
    )
endforeach()

install(
  FILES "${project_config}" "${version_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT "${targets_export_name}"
  NAMESPACE "${namespace}"
  DESTINATION "${config_install_dir}"
  )
