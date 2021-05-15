cmake_minimum_required (VERSION 3.12)
project (HDF5MyApp C CXX)

#set (LIB_TYPE STATIC) # or SHARED
set (LIB_TYPE SHARED) # or STATIC
string(TOLOWER ${LIB_TYPE} SEARCH_TYPE)

if (NOT HDF5_DIR)
  set(HDF5_DIR $ENV{HDF5_ROOT})
endif()

if (HDF5_DIR OR WIN32)
  # Find cmake build HDF5
  find_package (HDF5 NAMES hdf5 COMPONENTS C ${SEARCH_TYPE})
  set (LINK_LIBS ${LINK_LIBS} ${HDF5_C_${LIB_TYPE}_LIBRARY})
else()
  # Find non-cmake built HDF5
  find_package (HDF5)
  set_directory_properties(PROPERTIES INCLUDE_DIRECTORIES "${HDF5_INCLUDE_DIR}")
  set (LINK_LIBS ${LINK_LIBS} ${HDF5_LIBRARIES})
endif()

find_package(ZLIB REQUIRED)

set (example hdf_example)
add_executable (${example} ${PROJECT_SOURCE_DIR}/${example}.cxx)
target_link_libraries (${example} ${LINK_LIBS})
message(STATUS "LINK_LIBS=${LINK_LIBS}")

enable_testing ()
include (CTest)

add_test (NAME test_example COMMAND ${example})


get_cmake_property(_variableNames VARIABLES)
list (SORT _variableNames)
foreach (_variableName ${_variableNames})
  message(STATUS "${_variableName}=${${_variableName}}")
endforeach()
