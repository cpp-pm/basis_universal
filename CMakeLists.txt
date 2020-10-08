cmake_minimum_required(VERSION 3.1)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.273.tar.gz"
    SHA1 "43ffd06cf0352babdbe28d333adb15379ab59681"
)

project(basis_universal VERSION 1.12)

#option(BUILD_X64 "build 64-bit" TRUE)
#option(STATIC "static linking" FALSE)
option(BUILD_SHARED_LIBS "Build shared libs" OFF)
option(BASISU_BUILD_TOOL "Build the basisu encoding tool" ON)

#message("Initial BUILD_X64=${BUILD_X64}")
#message("Initial CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}")

#if(NOT CMAKE_BUILD_TYPE)
#  set(CMAKE_BUILD_TYPE Release)
#endif(NOT CMAKE_BUILD_TYPE)

#message(${PROJECT_NAME} " build type: " ${CMAKE_BUILD_TYPE})

#if (BUILD_X64)
#	message("Building 64-bit")
#else()
#	message("Building 32-bit")
#endif(BUILD_X64)

if(MSVC)
  add_compile_definitions(BASISU_NO_ITERATOR_DEBUG_LEVEL)
else()
#   set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")
#   set(CMAKE_C_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")
#
#   set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}")
#   set(CMAKE_C_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE}")
#
#   set(GCC_COMPILE_FLAGS "-fvisibility=hidden -fPIC -fno-strict-aliasing -D_LARGEFILE64_SOURCE=1 -D_FILE_OFFSET_BITS=64 -Wall -Wextra -Wno-unused-local-typedefs -Wno-unused-value -Wno-unused-parameter -Wno-unused-variable")
    set(GCC_COMPILE_FLAGS "-fvisibility=hidden -fno-strict-aliasing -D_LARGEFILE64_SOURCE=1 -D_FILE_OFFSET_BITS=64 -Wall -Wextra -Wno-unused-local-typedefs -Wno-unused-value -Wno-unused-parameter -Wno-unused-variable")
#
#   if (NOT BUILD_X64)
#	  set(GCC_COMPILE_FLAGS "${GCC_COMPILE_FLAGS} -m32")
#   endif()
#
   if (EMSCRIPTEN)
	  set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -s ALLOW_MEMORY_GROWTH=1")
	  set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -s ALLOW_MEMORY_GROWTH=1")

	  set(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} ${GCC_LINK_FLAGS}")
#   elseif (STATIC)
#	  set(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} ${GCC_LINK_FLAGS} -static-libgcc -static-libstdc++ -static")
#   else()
#	  set(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} ${GCC_LINK_FLAGS} -Wl,-rpath .")
   endif()

   set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} ${GCC_COMPILE_FLAGS}")
   #set(CMAKE_C_FLAGS_RELEASE  "${CMAKE_C_FLAGS_RELEASE} ${GCC_COMPILE_FLAGS}")
   #set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} ${GCC_COMPILE_FLAGS} -D_DEBUG")

   set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${GCC_COMPILE_FLAGS}")
   #set(CMAKE_CXX_FLAGS_RELEASE  "${CMAKE_CXX_FLAGS_RELEASE} ${GCC_COMPILE_FLAGS}")
   #set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} ${GCC_COMPILE_FLAGS} -D_DEBUG")
endif()

add_compile_definitions("$<$<CONFIG:DEBUG>:_DEBUG>")

if (APPLE)
   set(BIN_DIRECTORY "bin_osx")
else()
   set(BIN_DIRECTORY "bin")
endif()
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/${BIN_DIRECTORY})

# Introduce variables:
# * CMAKE_INSTALL_LIBDIR
# * CMAKE_INSTALL_BINDIR
# * CMAKE_INSTALL_INCLUDEDIR
include(GNUInstallDirs)

set(GENERATED_DIR "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(VERSION_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
set(PROJECT_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}Config.cmake")

set(CONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")

set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
set(NAMESPACE "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${VERSION_CONFIG}" COMPATIBILITY ExactVersion
)

configure_package_config_file(
    "cmake/Config.cmake.in"
    "${PROJECT_CONFIG}"
    INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}"
)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC AND BUILD_SHARED_LIBS)
   set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

include(3rdparty/CMakeLists.txt)
add_subdirectory(transcoder)
add_subdirectory(encoder)

if(BASISU_BUILD_TOOL)
	add_subdirectory(tool)
endif()

install(
    FILES "${PROJECT_CONFIG}" "${VERSION_CONFIG}"
    DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
    EXPORT "${TARGETS_EXPORT_NAME}"
    NAMESPACE "${NAMESPACE}"
    DESTINATION "${CONFIG_INSTALL_DIR}"
)
