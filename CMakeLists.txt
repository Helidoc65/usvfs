CMAKE_MINIMUM_REQUIRED(VERSION 3.0 FATAL_ERROR)

CMAKE_POLICY(SET CMP0056 NEW)

IF(DEFINED CMAKE_BUILD_TYPE)
   SET(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE} CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel.")
ELSE()
   SET(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel.")
ENDIF()

PROJECT(usvfs)

ENABLE_TESTING()

INCLUDE(${CMAKE_SOURCE_DIR}/StandardSetup.cmake)

INCLUDE(${CMAKE_SOURCE_DIR}/SetupPackage.cmake)

#INCLUDE(cotire)

SET(CMAKE_PREFIX_PATH "${QTDIR}; ${WINSDK_LIB}")

SET(PROJ_VERSION 0.2)
SET(PROJ_AUTHOR Tannin)

SET(PROJ_ARCH x86 CACHE STRING "build architecture, has to be x86 or x64")

ADD_DEFINITIONS(-DUNICODE -D_UNICODE -DASMJIT_STATIC -DPROJ_VERSION="${PROJ_VERSION}")

# optimizations to spdlog
# logger name isn't printed, loggers are created in advance
ADD_DEFINITIONS(-DSPDLOG_NO_NAME -DSPDLOG_NO_REGISTRY_MUTEX)

#ADD_DEFINITIONS(-DBOOST_EXCEPTION_DISABLE -DBOOST_NO_EXCEPTIONS)
SET(default_project_path "${CMAKE_SOURCE_DIR}/..")
GET_FILENAME_COMPONENT(${default_project_path} ${default_project_path} REALPATH)

SET(project_path "${default_project_path}" CACHE PATH "path to the other mo projects")
SET(lib_path "${CMAKE_INSTALL_PREFIX}/libs")
LINK_DIRECTORIES("${lib_path}")

FIND_PROGRAM(IWYU_PATH NAMES include-what-you-use iwyu PATHS ${project_path}/include-what-you-use/bin)
IF(IWYU_PATH-NOTFOUND)
  MESSAGE(STATUS "Could not find the program include-what-you-use")
ENDIF()

ADD_SUBDIRECTORY(shared)
ADD_SUBDIRECTORY(shared_test)
ADD_SUBDIRECTORY(thooklib)
ADD_SUBDIRECTORY(thooklib_test)
ADD_SUBDIRECTORY(tinjectlib)
ADD_SUBDIRECTORY(tinjectlib_test)
ADD_SUBDIRECTORY(testinject_dll)
ADD_SUBDIRECTORY(testinject_bin)
ADD_SUBDIRECTORY(usvfs)
ADD_SUBDIRECTORY(usvfs_test)
ADD_SUBDIRECTORY(usvfs_helper)
ADD_SUBDIRECTORY(usvfs_proxy)
