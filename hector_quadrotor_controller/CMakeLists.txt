cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#rosbuild_add_boost_directories()

set(MATLAB_MODEL_DIR "${CMAKE_SOURCE_DIR}/matlab/emcprj/rtwlib")
add_subdirectory(matlab)
add_library(quadrotorPropulsion STATIC IMPORTED GLOBAL)
add_dependencies(quadrotorPropulsion matlab_compile_all)
set_target_properties(quadrotorPropulsion PROPERTIES IMPORTED_LOCATION "${MATLAB_MODEL_DIR}/quadrotorPropulsion/quadrotorPropulsion.a")

add_library(quadrotorDrag STATIC IMPORTED GLOBAL)
add_dependencies(quadrotorDrag matlab_compile_all)
set_target_properties(quadrotorDrag PROPERTIES IMPORTED_LOCATION "${MATLAB_MODEL_DIR}/quadrotorDrag/quadrotorDrag.a")

include_directories(${MATLAB_MODEL_DIR})
rosbuild_add_library(hector_gazebo_quadrotor_aerodynamics src/quadrotor_aerodynamics.cpp)
#target_link_libraries(hector_gazebo_quadrotor_aerodynamics "${MATLAB_MODEL_DIR}/quadrotorPropulsion/quadrotorPropulsion.a" "${MATLAB_MODEL_DIR}/quadrotorDrag/quadrotorDrag.a")
#add_dependencies(hector_gazebo_quadrotor_aerodynamics quadrotorPropulsion quadrotorDrag)
target_link_libraries(hector_gazebo_quadrotor_aerodynamics quadrotorPropulsion quadrotorDrag)

rosbuild_add_executable(test_trajectory src/test_trajectory.cpp)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

#common commands for building c++ executables and libraries
#rosbuild_add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

