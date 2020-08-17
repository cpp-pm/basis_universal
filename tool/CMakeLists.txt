file(GLOB BASISU_TOOL_SRCS ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

add_executable(basisu_tool ${BASISU_TOOL_SRCS})
set_target_properties(basisu_tool PROPERTIES OUTPUT_NAME "basisu")
target_link_libraries(basisu_tool PRIVATE basisu_encoder basisu_transcoder)
target_include_directories(basisu_tool
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/..>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

if (NOT EMSCRIPTEN)
    if (UNIX)
        if (CMAKE_BUILD_TYPE STREQUAL Release)
            if (APPLE)
                add_custom_command(TARGET basisu POST_BUILD COMMAND strip -X -x ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/basisu)
            else()
                add_custom_command(TARGET basisu POST_BUILD COMMAND strip -g -X -x ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/basisu)
            endif()
        endif()
     endif()
 endif()

install(TARGETS basisu_tool DESTINATION "${CMAKE_INSTALL_BINDIR}")
