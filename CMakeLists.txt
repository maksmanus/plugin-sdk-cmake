cmake_minimum_required(VERSION 3.11)
project("plUGIN SDK CMAKE VERSION" LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 23)
add_compile_definitions(GTASA)
add_compile_definitions(RW)
add_compile_options(/MP)
add_compile_options(/w)

# find includes
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/game_sa)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/hooking)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/injector)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/stb)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/safetyhook)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/shared)

file(GLOB_RECURSE ALL_INCLUDE_DIRS LIST_DIRECTORIES true ${CMAKE_CURRENT_SOURCE_DIR}/shared/*)
include_directories(${ALL_INCLUDE_DIRS})

file(GLOB_RECURSE SHARED ${CMAKE_CURRENT_SOURCE_DIR}/shared/*.cpp)
file(GLOB_RECURSE GTASA_CPP ${CMAKE_CURRENT_SOURCE_DIR}/include/game_sa/*.cpp)

# adding sources in build
add_library(plugin-sdk STATIC 
    ${SHARED}
    ${GTASA_CPP}
    ${CMAKE_CURRENT_SOURCE_DIR}/hooking/Hooking.Patterns.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/safetyhook/safetyhook.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/safetyhook/Zydis.c)
    
# find libs files
file(GLOB_RECURSE LIBS ${CMAKE_CURRENT_SOURCE_DIR}/lib/*.lib)
target_link_libraries(plugin-sdk PRIVATE ${LIBS})
