# vim: set ts=8 noet:

project(luahidapi C)

cmake_minimum_required(VERSION 2.6)

# Stick to gcc on OSX as I can't grasp the clang's compiler messages yet
if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	SET(CMAKE_C_COMPILER /usr/bin/gcc)
	SET(CMAKE_CXX_COMPILER /usr/bin/g++)
endif()

set(LIB_VERSION_MAJOR "1")
set(LIB_VERSION_MINOR "0")
set(LIB_VERSION_PATCH "1")

set(LIBRARY_VERSION "${LIB_VERSION_MAJOR}.${LIB_VERSION_MINOR}.${LIB_VERSION_PATCH}")
set(LIBRARY_SOVERSION "0")

set(CMAKE_MODULE_PATH
	${CMAKE_SOURCE_DIR}/cmake
	${CMAKE_SOURCE_DIR}/cmake/modules)

include(DefineCMakeDefaults)
include(DefinePlatformDefaults)
include(DefineCompilerFlags)
include(DefineOptions)
include(CPackConfig)

include(DefineInstallationPaths)
# install .dlls to bin/
set(BIN_INSTALL_DIR "bin" CACHE PATH "-")

configure_file(${CMAKE_SOURCE_DIR}/src/version.h.cmake ${CMAKE_SOURCE_DIR}/src/version.h)
add_subdirectory(3rdparty)
add_subdirectory(src)

if(UNIT_TESTING)
	enable_testing()
	add_subdirectory(tests)
endif()
