add_library(jpgd STATIC ${CMAKE_CURRENT_SOURCE_DIR}/jpgd.h ${CMAKE_CURRENT_SOURCE_DIR}/jpgd.cpp)
target_include_directories(jpgd
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
