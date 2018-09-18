cmake_minimum_required(VERSION 2.8.3)
project(voxblox_python)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

set(CMAKE_MACOSX_RPATH 0)
add_definitions(-std=c++11 -Wall -Wextra)

if (EXISTS "${CMAKE_CURRENT_LIST_DIR}/../tools/pybind11/CMakeLists.txt")
  set(HAVE_PYBIND11 TRUE)
  message(STATUS "Found pybind11; generating Python bindings")
endif()

###################
# PYTHON BINDINGS #
###################
if(HAVE_PYBIND11)
  catkin_python_setup()

  add_subdirectory(../tools/pybind11 pybind11)
  message("Building Python bindings for voxblox")
  pybind11_add_module(voxbloxpy
    src/esdf_map_bind.cc
    src/tsdf_map_bind.cc
    src/esdf_integrator_bind.cc
    src/tsdf_integrator_bind.cc
    src/voxel_bind.cc
    src/layer_bind.cc
    src/block_bind.cc
    src/layer_io_bind.cc
    src/planning_utils_bind.cc
    src/bind.cc)
  set_target_properties(voxbloxpy PROPERTIES LINKER_LANGUAGE CXX)
  target_link_libraries(voxbloxpy PUBLIC ${catkin_LIBRARIES})

  set_target_properties(voxbloxpy
    PROPERTIES LIBRARY_OUTPUT_DIRECTORY
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_PYTHON_DESTINATION}/../)
endif()


##########
# EXPORT #
##########
cs_install()
cs_export()
