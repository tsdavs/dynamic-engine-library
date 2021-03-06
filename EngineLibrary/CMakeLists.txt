cmake_minimum_required(VERSION 3.10)
project (EngineLibrary)
set(CMAKE_CXX_STANDARD 11)
set( CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall --std=c++11 -O3 -fPIC" )
set( CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wall -O3 -fPIC" )

find_package(OpenGL REQUIRED)
message(STATUS "OpenGL included at ${OPENGL_INCLUDE_DIR}")

# Compile external dependencies 
add_subdirectory (external)

cmake_policy(SET CMP0026 OLD)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/external/rpavlik-cmake-modules-fe2273")

if(INCLUDE_DISTRIB)
        add_subdirectory(distrib)
endif(INCLUDE_DISTRIB)

include_directories(
        external/AntTweakBar-1.16/include/
        external/glfw-3.1.2/include/
        external/glm-0.9.7.1/
        external/glew-1.13.0/include/
        external/assimp-3.0.1270/include/
        external/bullet-2.81-rev2613/src/
        .
)

set(ALL_LIBS
        ${OPENGL_LIBRARY}
        glfw
        GLEW_1130
)

add_definitions(
        -DTW_STATIC
        -DTW_NO_LIB_PRAGMA
        -DTW_NO_DIRECT3D
        -DGLEW_STATIC
        -D_CRT_SECURE_NO_WARNINGS
)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_library(EngineLibrary SHARED 
	src/Initialise.cpp

        common/shader.cpp
        common/shader.hpp
        common/texture.cpp
        common/texture.hpp
        common/controls.cpp
        common/controls.hpp
	)

TARGET_LINK_LIBRARIES(EngineLibrary ${ALL_LIBS})

install(TARGETS EngineLibrary DESTINATION /usr/lib)
install(FILES 
	include/Initialise.h 
	DESTINATION include
	)