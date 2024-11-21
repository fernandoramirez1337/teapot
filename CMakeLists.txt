cmake_minimum_required(VERSION 3.5)

project(rubikscube)

find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS})

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Adding GLM
find_package(glm CONFIG REQUIRED)
include_directories(${GLM_INCLUDE_DIRS})

# Add include directories
include_directories("${GLFW_SOURCE_DIR}/deps")
# include_directories(${CMAKE_SOURCE_DIR}/include)
# include_directories(${CMAKE_SOURCE_DIR}/src)

set(GLFW_BUILD_DOCS OFF CACHE BOOL "GLFW lib only")
set(GLFW_INSTALL OFF CACHE BOOL "GLFW lib only")
set(GLAD_GL "${GLFW_SOURCE_DIR}/deps/glad/gl.h")
set(GLAD_GLES2 "${GLFW_SOURCE_DIR}/deps/glad/gles2.h")

add_subdirectory(glfw)

if(MSVC)
    SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /ENTRY:mainCRTStartup")
endif()

# List source files
file(GLOB SOURCES
    "${CMAKE_SOURCE_DIR}/*.cpp"
)

set(GLFW-CMAKE-STARTER-SRC
    main.cpp
)

# Define executable before linking libraries
add_executable(rubikscube WIN32 ${GLFW-CMAKE-STARTER-SRC} ${SOURCES} ${GLAD_GL})

# Link libraries after the target has been defined
target_link_libraries(rubikscube ${OPENGL_LIBRARIES} glfw glm::glm)

if(MSVC)
    if(${CMAKE_VERSION} VERSION_LESS "3.6.0")
        message("\n\t[ WARNING ]\n\n\tCMake version lower than 3.6.\n\n\t - Please update CMake and rerun; OR\n\t - Manually set 'rubikscube' as StartUp Project in Visual Studio.\n")
    else()
        set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT rubikscube)
    endif()
endif()