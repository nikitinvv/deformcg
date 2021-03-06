enable_language(CUDA)
include_directories(${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})

# These policies are related to SWIG python libraries naming
cmake_policy(SET CMP0078 NEW)
cmake_policy(SET CMP0086 NEW)
find_package(SWIG REQUIRED)
include(${SWIG_USE_FILE})

# SWIG requires the Python header
find_package(PythonLibs REQUIRED)
include_directories(${PYTHON_INCLUDE_PATH})
# TODO: Is NumPy required?
find_package(NumPy REQUIRED)
include_directories(${NumPy_INCLUDE_DIRS})

# Must tell CMake that SWIG interface files are CXX files
set_source_files_properties(deform.i PROPERTIES CPLUSPLUS ON)
swig_add_library(deform LANGUAGE python SOURCES deform.i deform.cu)
swig_link_libraries(deform cudart cufft nppig nppial nppc npps nppidei)

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/deform.py" DESTINATION deformcg)
install(TARGETS deform LIBRARY DESTINATION deformcg)
