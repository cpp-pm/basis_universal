add_library(lodepng STATIC ${CMAKE_CURRENT_SOURCE_DIR}/lodepng.h ${CMAKE_CURRENT_SOURCE_DIR}/lodepng.cpp)
target_include_directories(lodepng
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
