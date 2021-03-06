cmake_minimum_required(VERSION 2.8)
project(pamix)

include(CMakeGitDefines.cmake)

option(WITH_UNICODE "Compile with unicode support and symbols" ON)
set(NCURSESW_H_INCLUDE "ncursesw/ncurses.h" CACHE STRING "ncursesw include directive content")

configure_file(config.hpp.in ${CMAKE_CURRENT_SOURCE_DIR}/config.hpp )
configure_file(man/pamix.1.in ${CMAKE_CURRENT_SOURCE_DIR}/man/pamix.1)

set(CMAKE_CXX_STANDARD 11)
file(GLOB_RECURSE pamix_SRC
        "include/*.h"
        "src/*.cpp")

include_directories("include")
link_libraries("pulse" "pthread")

IF (WITH_UNICODE)
    link_libraries("ncursesw")
    add_definitions(-DFEAT_UNICODE)
ELSE ()
    link_libraries("ncurses")
ENDIF ()

add_executable(pamix ${pamix_SRC})
install(FILES pamix.conf DESTINATION /etc/xdg)
install(TARGETS pamix DESTINATION bin)
install(FILES man/pamix.1 DESTINATION share/man/man1)

