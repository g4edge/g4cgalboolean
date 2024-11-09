#----------------------------------------------------------------------------
# Setup the project
cmake_minimum_required(VERSION 3.16...3.21)
project(geant4cgalboolean)

#----------------------------------------------------------------------------
# Find Geant4 package, activating all available Vis drivers by default
# You can set WITH_GEANT4_VIS to OFF via the command line or ccmake/cmake-gui
# to build a batch mode only executable
#
option(WITH_GEANT4_VIS "Build example with Geant4 Vis drivers" ON)
if(WITH_GEANT4_VIS)
  find_package(Geant4 REQUIRED gdml vis_all)
else()
  find_package(Geant4 REQUIRED gdml)
endif()

#----------------------------------------------------------------------------
# Add external 3rd party libraries
#

#----------------------------------------------------------------------------
# Setup Geant4 include directories and compile definitions
#
include(${Geant4_USE_FILE})

#----------------------------------------------------------------------------
# Setup Geant4 include directories and compile definitions
find_package(CGAL 5 REQUIRED)
find_package(Catch2 REQUIRED)
find_package(Eigen3 REQUIRED NO_MODULE)
#----------------------------------------------------------------------------
# Locate sources and headers for this project
#
include_directories(${PROJECT_SOURCE_DIR}/include 
                    ${Geant4_INCLUDE_DIR})

file(GLOB CGALLibSources ${PROJECT_SOURCE_DIR}/src/*cc)
file(GLOB CGALLibHeaders ${PROJECT_SOURCE_DIR}/include/*.hh)
add_library(g4cgalboolean SHARED ${CGALLibSources} ${CGALLibHeaders})
target_compile_options(g4cgalboolean PRIVATE -Werror )
target_link_libraries(g4cgalboolean ${Geant4_LIBRARIES} CGAL::CGAL Eigen3::Eigen)

file(GLOB TestSources ${PROJECT_SOURCE_DIR}/test/*cc)
file(GLOB TestHeaders ${PROJECT_SOURCE_DIR}/test/*hh)
add_executable(g4cgalbool_test ${TestSources} ${TestHeaders})
target_compile_options(g4cgalbool_test PRIVATE -Werror)
target_link_libraries(g4cgalbool_test ${Geant4_LIBRARIES} CGAL::CGAL g4cgalboolean)


include(CMakePackageConfigHelpers)
set(INCLUDE_INSTALL_DIR ${CMAKE_INSTALL_INCLUDEDIR}
        CACHE PATH "Location of header files" )
set(LIBRARY_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR}
        CACHE PATH "Location of library files" )
set(SYSCONFIG_INSTALL_DIR ${CMAKE_INSTALL_SYSCONFDIR}
        CACHE PATH "Location of configuration files" )
configure_package_config_file(cmake/geant4cgalboolean.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/geant4cgalboolean.cmake
        INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/geant4cgalboolean
        PATH_VARS INCLUDE_INSTALL_DIR SYSCONFIG_INSTALL_DIR LIBRARY_INSTALL_DIR)
write_basic_package_version_file(
        ${CMAKE_CURRENT_BINARY_DIR}/geant4cgalbooleanVersion.cmake
        VERSION 1.0
        COMPATIBILITY SameMajorVersion )
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/geant4cgalboolean.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/geant4cgalbooleanVersion.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/geant4cgalboolean)

install(TARGETS g4cgalboolean)
install(FILES ${CGALLibHeaders} DESTINATION include)
