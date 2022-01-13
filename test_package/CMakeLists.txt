#
# Copyright(c) 2020 ADLINK Technology Limited and others
#
# This program and the accompanying materials are made available under the
# terms of the Eclipse Public License v. 2.0 which is available at
# http://www.eclipse.org/legal/epl-2.0, or the Eclipse Distribution License
# v. 1.0 which is available at
# http://www.eclipse.org/org/documents/edl-v10.php.
#
# SPDX-License-Identifier: EPL-2.0 OR BSD-3-Clause
#
project(helloworld LANGUAGES C CXX)
cmake_minimum_required(VERSION 3.5)

if(EXISTS "${CMAKE_BINARY_DIR}/conan_paths.cmake")
  include(${CMAKE_BINARY_DIR}/conan_paths.cmake)
endif()

find_package(CycloneDDS-CXX MODULE REQUIRED)
#find_package(CycloneDDS-CXX CONFIG REQUIRED)

idlcxx_generate(TARGET helloworlddata FILES HelloWorldData.idl)

add_executable(ddscxxHelloworldPublisher publisher.cpp)
add_executable(ddscxxHelloworldSubscriber subscriber.cpp)

# Link both executables to idl data type library and ddscxx.
target_link_libraries(ddscxxHelloworldPublisher CycloneDDS-CXX::ddscxx helloworlddata)
target_link_libraries(ddscxxHelloworldSubscriber CycloneDDS-CXX::ddscxx helloworlddata)

target_compile_options(ddscxxHelloworldPublisher PRIVATE -fno-analyzer)
target_compile_options(ddscxxHelloworldSubscriber PRIVATE -fno-analyzer)

set_property(TARGET ddscxxHelloworldPublisher PROPERTY CXX_STANDARD 17)
set_property(TARGET ddscxxHelloworldSubscriber PROPERTY CXX_STANDARD 17)