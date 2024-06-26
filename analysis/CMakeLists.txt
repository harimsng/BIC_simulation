project(analysis)

find_package(ROOT REQUIRED)

set(CMAKE_MACOSX_RPATH 1)
list(APPEND CMAKE_INSTALL_RPATH ${CMAKE_PREFIX_PATH}/lib)
list(APPEND CMAKE_INSTALL_RPATH /opt/homebrew/lib/root)
list(APPEND CMAKE_INSTALL_RPATH /Users/hseong/eic/DRC_generic_forEIC/install/lib)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}/include
  # ${FASTJET_DIR}/include
  # ${HEPMC_DIR}/include
)
file(GLOB sources
  ${PROJECT_SOURCE_DIR}/src/*.cc
)
file(GLOB headers
  ${PROJECT_SOURCE_DIR}/include/*.h
)

include(${ROOT_USE_FILE})
add_executable(analysis analysis.cc ${sources} ${headers})
# add_executable(JER JER.cc ${sources} ${headers})
# add_executable(calib calib.cc ${sources} ${headers})
target_link_libraries(
  analysis
  # ${HEPMC_DIR}/lib64/libHepMC3.so
  # ${HEPMC_DIR}/lib64/libHepMC3rootIO.so
  rootIO
  ${ROOT_LIBRARIES}
  ${CMAKE_DL_LIBS}
)
# target_link_libraries(
#   JER
#   ${HEPMC_DIR}/lib64/libHepMC3.so
#   ${HEPMC_DIR}/lib64/libHepMC3rootIO.so
#   rootIO
#   ${ROOT_LIBRARIES}
#   ${CMAKE_DL_LIBS}
# )
# target_link_libraries(
#   calib
#   ${HEPMC_DIR}/lib64/libHepMC3.so
#   ${HEPMC_DIR}/lib64/libHepMC3rootIO.so
#   rootIO
#   ${ROOT_LIBRARIES}
#   ${CMAKE_DL_LIBS}
# )

set(
  analysis_SCRIPTS
)

foreach(_script ${analysis_SCRIPTS})
  configure_file(
    ${PROJECT_SOURCE_DIR}/${_script}
    ${PROJECT_BINARY_DIR}/${_script}
    COPYONLY
  )
endforeach()

# install(TARGETS analysis JER calib DESTINATION bin)
# install(TARGETS analysis JER DESTINATION bin)
install(TARGETS analysis DESTINATION bin)
