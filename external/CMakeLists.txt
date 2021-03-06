# -----------------------------------------------------------------------------
# Numenta Platform for Intelligent Computing (NuPIC)
# Copyright (C) 2015, Numenta, Inc.  Unless you have purchased from
# Numenta, Inc. a separate commercial license for this software code, the
# following terms and conditions apply:
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Affero Public License version 3 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
# See the GNU Affero Public License for more details.
#
# You should have received a copy of the GNU Affero Public License
# along with this program.  If not, see http://www.gnu.org/licenses.
#
# http://numenta.org/licenses/
# -----------------------------------------------------------------------------

cmake_minimum_required(VERSION 2.8)
project(nupic_core_main CXX)

set(CMAKE_VERBOSE_MAKEFILE OFF)

set_directory_properties(PROPERTIES EP_BASE "${EP_BASE}")

# Shorter aliases for static library prefix and suffix.
set(STATIC_PRE ${CMAKE_STATIC_LIBRARY_PREFIX})
set(STATIC_SUF ${CMAKE_STATIC_LIBRARY_SUFFIX})
# This is the library prefix path.
set(LIB_PRE ${EP_BASE}/Install/lib)
# This is the include path.
set(INCLUDE_PRE ${EP_BASE}/Install/include)
# The full prefix path for binaries.
set(BIN_PRE ${EP_BASE}/Install/bin)

include(ExternalProject)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}")

include(CapnProto)

include(Swig)

include(Apr1Lib)
set(LIB_STATIC_APR1_LOC ${LIB_STATIC_APR1_LOC} PARENT_SCOPE)
set(LIB_STATIC_APR1_INC_DIR ${LIB_STATIC_APR1_INC_DIR} PARENT_SCOPE)
list(APPEND EXTERNAL_INCLUDE_DIRS
     ${LIB_STATIC_APR1_INC_DIR}
     "${LIB_STATIC_APR1_INC_DIR}/apr-1"
)

# AprUtil1Lib MUST be imported after Apr1Lib due to depencies on exported
# properties
include(AprUtil1Lib)
set(LIB_STATIC_APRUTIL1_LOC ${LIB_STATIC_APRUTIL1_LOC} PARENT_SCOPE)
set(LIB_STATIC_APRUTIL1_INC_DIR ${LIB_STATIC_APRUTIL1_INC_DIR} PARENT_SCOPE)
list(APPEND EXTERNAL_INCLUDE_DIRS ${LIB_STATIC_APRUTIL1_INC_DIR})

include(YamlLib)
set(LIB_STATIC_YAML_LOC ${LIB_STATIC_YAML_LOC} PARENT_SCOPE)

include(YamlCppLib)
set(LIB_STATIC_YAML_CPP_LOC ${LIB_STATIC_YAML_CPP_LOC} PARENT_SCOPE)
set(LIB_STATIC_YAML_CPP_INC_DIR ${LIB_STATIC_YAML_CPP_INC_DIR} PARENT_SCOPE)
list(APPEND EXTERNAL_INCLUDE_DIRS ${LIB_STATIC_YAML_CPP_INC_DIR})

include(Zlib)
set(LIB_STATIC_Z_LOC ${LIB_STATIC_Z_LOC} PARENT_SCOPE)
set(LIB_STATIC_Z_INC_DIR ${LIB_STATIC_Z_INC_DIR} PARENT_SCOPE)
list(APPEND EXTERNAL_INCLUDE_DIRS ${LIB_STATIC_Z_INC_DIR})

# Add vendored prebuilt library include paths.
list(APPEND EXTERNAL_INCLUDE_DIRS
     "${PROJECT_SOURCE_DIR}/common/include")

# Convenience variable that wraps all external include directories.
list(APPEND EXTERNAL_INCLUDE_DIRS ${CAPNP_INCLUDE_DIRS})

set(EXTERNAL_INCLUDE_DIRS ${EXTERNAL_INCLUDE_DIRS} PARENT_SCOPE)
