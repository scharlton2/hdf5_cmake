cmake_minimum_required (VERSION 3.12)
project (HDF5MyApp C CXX)

set (LIB_TYPE STATIC) # or SHARED
string(TOLOWER ${LIB_TYPE} SEARCH_TYPE)

find_package (HDF5 NAMES hdf5 COMPONENTS C ${SEARCH_TYPE})
# find_package (HDF5) # Find non-cmake built HDF5
set_directory_properties(PROPERTIES INCLUDE_DIRECTORIES "${HDF5_INCLUDE_DIR}")
set (LINK_LIBS ${LINK_LIBS} ${HDF5_C_${LIB_TYPE}_LIBRARY})

set (example hdf_example)

add_executable (${example} ${PROJECT_SOURCE_DIR}/${example}.cxx)
##TARGET_C_PROPERTIES (${example} PRIVATE ${LIB_TYPE})
target_link_libraries (${example} ${LINK_LIBS})

enable_testing ()
include (CTest)

add_test (NAME test_example COMMAND ${example})