cmake_minimum_required(VERSION 3.26)
project(sparse-voxel-octrees)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})

# set C++11
set(CMAKE_CXX_STANDARD 11)

if (MSVC)
    add_definitions(-DNOMINMAX -D_CRT_SECURE_NO_WARNINGS)
endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${Vc_ARCHITECTURE_FLAGS}")

if (CMAKE_COMPILER_IS_GNUCXX)
    set(CXX_WARNINGS "-Wall -Wextra -Wpointer-arith -Wcast-align -fstrict-aliasing -Wno-unused-local-typedefs")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CXX_WARNINGS} -fvisibility-inlines-hidden")
endif()

find_package(SDL2 REQUIRED)

file(GLOB_RECURSE Sources "src/*.hpp" "src/*.cpp" "src/*.c")

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    #set(Sources ${Sources} "src/SDLMain.m")
endif()

if (WIN32)
    add_executable(sparse-voxel-octrees WIN32 ${Sources})
else()
    add_executable(sparse-voxel-octrees ${Sources})
endif()

# on Apple, we need to link against Cocoa
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    # find Cocoa
    find_library(COCOA_LIBRARY Cocoa)
    target_link_libraries(sparse-voxel-octrees PRIVATE ${COCOA_LIBRARY})
    endif()

target_include_directories(sparse-voxel-octrees PRIVATE ${SDL2_INCLUDE_DIRS})
target_include_directories(sparse-voxel-octrees PRIVATE src)
target_link_libraries(sparse-voxel-octrees PRIVATE ${SDL2_LIBRARIES})



set_target_properties(sparse-voxel-octrees PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/bin)
set_target_properties(sparse-voxel-octrees PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/bin)