cmake_minimum_required(VERSION 3.7)
project(BosmaScheduler VERSION 1.0.0 LANGUAGES CXX)

# Include
include(GNUInstallDirs)
include(ExternalProject)

# Threads
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

add_library(${PROJECT_NAME} INTERFACE)
target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_11)

target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

install(TARGETS ${PROJECT_NAME} EXPORT BosmaSchedulerConfig
    ARCHIVE  DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY  DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME  DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})

install(EXPORT BosmaSchedulerConfig DESTINATION share/BosmaScheduler/cmake)

export(TARGETS ${PROJECT_NAME} FILE BosmaSchedulerConfig.cmake)
