cmake_minimum_required(VERSION 3.14)

include(FetchContent)

project(bgl_wrapper)


set(CMAKE_CXX_FLAGS "-Wall -Wextra -O3")
set(CMAKE_OSX_ARCHITECTURES "x86_64")
set(PYBIND_SRC_DIR pybind11)
set(BOOST_DIR boost_1_80_0)

if(NOT EXISTS ${BOOST_DIR})
  file(DOWNLOAD https://boostorg.jfrog.io/artifactory/main/release/1.80.0/source/boost_1_80_0.tar.gz boost.tar.gz EXPECTED_MD5 077f074743ea7b0cb49c6ed43953ae95)
  execute_process(COMMAND tar xvf boost.tar.gz)
endif()

include_directories(${CMAKE_CURRENT_BINARY_DIR}/${BOOST_DIR})

if(NOT EXISTS ${PYBIND_SRC_DIR})
  execute_process(COMMAND git clone --branch v2.6 https://github.com/pybind/pybind11.git ${PYBIND_SRC_DIR})
endif()

add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/${PYBIND_SRC_DIR})

pybind11_add_module(bgl_wrapper bgl_wrapper.cpp)