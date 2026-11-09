# CMake project for ElectronicStore DLL
cmake_minimum_required(VERSION 3.10)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project("ElectronicStore")

# Build as a shared library (DLL)
add_library(ElectronicStore SHARED "dllmain.cpp")

# C++23 standard
if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET ElectronicStore PROPERTY CXX_STANDARD 23)
  set_property(TARGET ElectronicStore PROPERTY CXX_STANDARD_REQUIRED ON)
endif()
