add_library(apg_bmp STATIC ${CMAKE_CURRENT_SOURCE_DIR}/apg_bmp.h ${CMAKE_CURRENT_SOURCE_DIR}/apg_bmp.c)
target_include_directories(apg_bmp
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
