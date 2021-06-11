cmake_minimum_required(VERSION 3.10)
set(PROJECT "gst-zeromq")

set(GST_LICENSE "LGPL")
set(VERSION 1.0.0)
set(PACKAGE "${PROJECT}")

project("${PROJECT}" VERSION "${VERSION}")

find_package(PkgConfig REQUIRED)

pkg_check_modules(GST REQUIRED IMPORTED_TARGET
    "gstreamer-1.0"
    "gstreamer-base-1.0"
    "gstreamer-controller-1.0"
)

pkg_check_modules(ZMQ REQUIRED IMPORTED_TARGET
    "libzmq"
)

set(SRC_FILES 
    "${CMAKE_CURRENT_SOURCE_DIR}/src/zeromq/gstzmqplugin.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/zeromq/gstzmqsink.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/zeromq/gstzmqsrc.c"
)

set(TARGET "gstzmq")

set(INCLUDE_DIRS 
    "${CMAKE_CURRENT_SOURCE_DIR}/src/zeromq" 
)

add_library("${TARGET}" SHARED ${SRC_FILES})

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/cmake_config.h.in" "${CMAKE_CURRENT_SOURCE_DIR}/src/zeromq/config.h" @ONLY)
target_compile_definitions("${TARGET}" PRIVATE "HAVE_CONFIG_H")

target_include_directories("${TARGET}" PRIVATE ${INCLUDE_DIRS})


target_link_libraries("${TARGET}" PUBLIC PkgConfig::GST)
target_include_directories("${TARGET}" PUBLIC ${GST_INCLUDE_DIRS})

target_link_libraries("${TARGET}" PUBLIC PkgConfig::ZMQ)
target_include_directories("${TARGET}" PUBLIC ${ZMQ_INCLUDE_DIRS})

pkg_get_variable(INSTALL_DIR "gstreamer-1.0" "pluginsdir")
install(TARGETS "${TARGET}" LIBRARY DESTINATION "${INSTALL_DIR}")