cmake_minimum_required(VERSION 2.8.3)
project(eigen_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/eigen3)

ExternalProject_Add(eigen_src
  URL https://github.com/ethz-asl/thirdparty_library_binaries/raw/master/Eigen_3.2.7.tar.gz
  UPDATE_COMMAND ""
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DCMAKE_BUILD_TYPE:STRING=Release
)

ExternalProject_CatkinInstall(eigen_src)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} eigen_src)

cs_install()
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/eigen3/
          CFG_EXTRAS eigen-extras.cmake)
