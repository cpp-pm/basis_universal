file(GLOB BASISU_ENCODER_HDRS ${CMAKE_CURRENT_SOURCE_DIR}/*.h ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp)
file(GLOB BASISU_ENCODER_SRCS ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

add_library(basisu_encoder ${BASISU_ENCODER_SRCS} ${BASISU_ENCODER_HDRS})
target_link_libraries(basisu_encoder basisu_transcoder LodePNG::lodepng apg::bmp jpeg-compressor::jpgd miniz::miniz)
target_include_directories(basisu_encoder
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}>)

find_package(Threads REQUIRED)
target_link_libraries(basisu_encoder Threads::Threads)
if (NOT MSVC)
    target_link_libraries(basisu_encoder m)
endif()

install(TARGETS basisu_encoder
    EXPORT "${TARGETS_EXPORT_NAME}"
    INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}"
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}")
install(FILES ${BASISU_ENCODER_HDRS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
