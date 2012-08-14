file(GLOB MFILES "*.m")
add_custom_command(
  OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/emcprj/rtwlib/quadrotorPropulsion/quadrotorPropulsion.a ${CMAKE_CURRENT_SOURCE_DIR}/emcprj/rtwlib/quadrotorDrag/quadrotorDrag.a
  COMMAND matlab -nodesktop -nosplash -r "compile_all; quit;"
  DEPENDS ${MFILES}
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
  VERBATIM
)

add_custom_target(matlab_compile_all
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/emcprj/rtwlib/quadrotorPropulsion/quadrotorPropulsion.a ${CMAKE_CURRENT_SOURCE_DIR}/emcprj/rtwlib/quadrotorDrag/quadrotorDrag.a
  SOURCES ${MFILES}
)

set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES ${CMAKE_CURRENT_SOURCE_DIR}/emcprj)
