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

project(Bullet VERSION 1.0.0)

# Define target name
set (TARGET_NAME Bullet)

# Define source files
file (GLOB CPP_FILES src/BulletCollision/BroadphaseCollision/*.cpp 
  src/BulletCollision/CollisionDispatch/*.cpp src/BulletCollision/CollisionShapes/*.cpp 
  src/BulletCollision/Gimpact/*.cpp src/BulletCollision/NarrowPhaseCollision/*.cpp 
  src/BulletDynamics/Character/*.cpp src/BulletDynamics/ConstraintSolver/*.cpp
  src/BulletDynamics/Dynamics/*.cpp src/BulletDynamics/Featherstone/*.cpp 
  src/BulletDynamics/MLCPSolvers/*.cpp src/BulletDynamics/Vehicle/*.cpp src/BulletSoftBody/*.cpp
  src/LinearMath/*.cpp)
file (GLOB H_FILES *.h src/BulletCollision/BroadphaseCollision/*.h
  src/BulletCollision/CollisionDispatch/*.h src/BulletCollision/CollisionShapes/*.h
  src/BulletCollision/Gimpact/*.h src/BulletCollision/NarrowPhaseCollision/*.h
  src/BulletDynamics/Character/*.h src/BulletDynamics/ConstraintSolver/*.h
  src/BulletDynamics/Dynamics/*.h src/BulletDynamics/Featherstone/*.h 
  src/BulletDynamics/MLCPSolvers/*.h src/BulletDynamics/Vehicle/*.h src/BulletSoftBody/*.h
  src/LinearMath/*.h)
set (SOURCE_FILES ${CPP_FILES} ${H_FILES})

include_directories(src)

add_library(Bullet ${SOURCE_FILES})

#### install 

set(TARGET_PUBLIC_HDRS "${H_FILES}")

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
  TARGETS Bullet
  EXPORT "${targets_export_name}"
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin"
  INCLUDES DESTINATION "${include_install_dir}"
  )

install(
  DIRECTORY src/ # Trailing slash is required
  DESTINATION "${include_install_dir}/${PROJECT_NAME}"
  FILES_MATCHING PATTERN "*.h" PATTERN "*.hpp"
  )

install(
  FILES "${project_config}" "${version_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT "${targets_export_name}"
  NAMESPACE "${namespace}"
  DESTINATION "${config_install_dir}"
  )