cmake_minimum_required( VERSION 2.8 FATAL_ERROR )
project (urdf_parser_py)

find_program(PYTHON "python")

if (NOT PYTHON)
  return()
endif()

set(SETUP_PY "${CMAKE_CURRENT_SOURCE_DIR}/setup.py")
install(CODE "execute_process(COMMAND \"${PYTHON}\" \"${SETUP_PY}\" build --build-base \"${CMAKE_CURRENT_BINARY_DIR}/pybuild\" install --install-layout deb --prefix \"${CMAKE_INSTALL_PREFIX}\"
              WORKING_DIRECTORY \"${CMAKE_CURRENT_SOURCE_DIR}\")")

enable_testing()

add_test(NAME urdf_parser_py
         COMMAND /usr/bin/env PYTHONPATH=${CMAKE_CURRENT_SOURCE_DIR}/src ${PYTHON} test_urdf.py
         WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/test)

add_test(NAME urdf_parser_py_error
         COMMAND /usr/bin/env PYTHONPATH=${CMAKE_CURRENT_SOURCE_DIR}/src ${PYTHON} test_urdf_error.py
         WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/test)
