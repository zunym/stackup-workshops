#
# Generated CMakeLists.txt for compiling the classifier_16k module on pi3
# WARNING: Any changes made to this file will be overwritten!
#
# Required variables for configuring this file from CMake:
#    ELL_model - name of the model
#    Arch - target architecture, e.g. host, pi3.
#    OBJECT_EXTENSION - obj or o
#    ELL_ROOT - location of ELL git repo
#
# Usage:
#
#   Linux / Mac:
#     mkdir build && cd build
#     cmake ..
#     make
#
#   Windows 64 bit:
#     mkdir build && cd build
#     VS 2015: cmake -G "Visual Studio 14 2015 Win64" ..
#     VS 2017: cmake -G "Visual Studio 15 2017 Win64" ..
#     Build the generated .sln in Release, or run cmake --build . --config=Release
#
#
#  Loading module from python:
#
#  From the parent directory (where the generated model directory was created)
#    >>> from classifier_16k import model

cmake_minimum_required(VERSION 3.3)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(classifier_16k)
set(module_prefix)

# dependencies
if(WIN32)
    # path to the OpenBLAS Nuget
    set(PACKAGE_ROOT "/ELL/external")
else()
    # SWIG has this GCC 7 warning.
    add_compile_options("-Wno-psabi")    
endif()
include(OpenBLASSetup.cmake)

find_package(PythonInterp 3.4)
find_package(PythonLibs 3.4)
if(${PYTHONLIBS_FOUND})

    # build the python module
    set(module_prefix _)

    set(target_name ${module_prefix}model)

    add_definitions(-DSWIG_PYTHON_INTERPRETER_NO_DEBUG)
    set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    add_library(${target_name} SHARED
        classifier_16k.obj
        classifier_16kPYTHON_wrap.cxx
        classifier_16kPYTHON_wrap.h
        include/CallbackInterface.h
        tcc/CallbackInterface.tcc)

    if(WIN32)
        set_property(TARGET ${target_name} PROPERTY SUFFIX ".pyd")
    else()
        set_property(TARGET ${target_name} PROPERTY PREFIX "")
    endif()

    if(APPLE)
        set_property(TARGET ${target_name} PROPERTY SUFFIX ".so")
    endif()

    target_include_directories(${target_name} PRIVATE include ${PYTHON_INCLUDE_PATH} ${BLAS_INCLUDE_DIRS})
    target_link_libraries(${target_name} PRIVATE ${PYTHON_LIBRARIES} ${BLAS_LIBS})

else()
  message(WARNING "Couldn't find Python libraries 3.4 or later, python module will not be created")
endif() # PYTHONLIBS_FOUND
