#  ----------------------------------------------------------------------------
#  2020 Bernd Pfrommer bernd.pfrommer@gmail.com
#
cmake_minimum_required(VERSION 3.5)
project(vilib_ament LANGUAGES CXX CUDA)

find_package(ament_cmake REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(CUDA REQUIRED)

add_subdirectory(vilib)


add_library(${PROJECT_NAME} INTERFACE)
add_dependencies(${PROJECT_NAME} vilib)

target_link_libraries(${PROJECT_NAME} INTERFACE Eigen3::Eigen vilib::vilib ${CUDA_LIBRARIES})

target_include_directories(${PROJECT_NAME}
  INTERFACE $<INSTALL_INTERFACE:include/vilib>
  ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES}
  ${CUDA_INCLUDE_DIRS}
)

install(
  TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  DESTINATION lib
  INCLUDES DESTINATION include
)

ament_export_targets(export_${PROJECT_NAME})

# need export_dependencies() or else you have to
# have find_package(vilib) in your package to
# use vlib_ament
ament_export_dependencies(vilib)

ament_package()
