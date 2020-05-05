cmake_minimum_required(VERSION 3.7)
project(scheduler VERSION 1.0.0 LANGUAGES CXX DESCRIPTION "Modern C++17 header-only scheduling library")
set(CMAKE_CXX_STANDARD 17)

include(GNUInstallDirs)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/libs/croncpp)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/libs/ctpl)

set(SOURCE_HEADERS
    include/cron.hpp
    include/interruptable_sleep.hpp
    include/scheduler.hpp
)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
target_link_libraries(${PROJECT_NAME}
    INTERFACE Threads::Threads
    INTERFACE croncpp
    INTERFACE ctpl
)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    RUNTIME DESTINATION ${CMAKE_INSTALL_LIBEXECDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME}
)
install(FILES ${SOURCE_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
