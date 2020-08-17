file(GLOB BASISU_ENCODER_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/*.h ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp)
file(GLOB BASISU_ENCODER_SRCS ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

add_library(basisu_encoder ${BASISU_ENCODER_SRCS} ${BASISU_ENCODER_HDRS})
target_link_libraries(basisu_encoder basisu_transcoder lodepng apg_bmp jpgd)
target_include_directories(basisu_encoder
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/..
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

if (NOT MSVC)
    find_package(Threads REQUIRED)
    target_link_libraries(basisu_encoder m Threads::Threads)
endif()

install(TARGETS basisu_encoder
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}")
install(FILES ${BASISU_ENCODER_HDRS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
