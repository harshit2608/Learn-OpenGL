cmake_minimum_required(VERSION 3.15)

set(PROJECT_NAME MightRenderer)
project(${PROJECT_NAME} LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
# set(CMAKE_CXX_EXTENSIONS OFF)

add_subdirectory(vendor)
add_subdirectory(MightRenderer)

file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if(EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with a CMakeLists.txt file). Please make a build subdirectory. Feel free to remove CMakeCache.txt and CMakeFiles.")
endif()

# Git submodules
find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${PROJECT_SOURCE_DIR}/.git")
# Update submodules as needed
    option(GIT_SUBMODULE "Check submodules during build" ON)
    if(GIT_SUBMODULE)
        message(STATUS "Submodule update")
        execute_process(COMMAND ${GIT_EXECUTABLE} submodule update --init --recursive
                        WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                        RESULT_VARIABLE GIT_SUBMOD_RESULT)
        if(NOT GIT_SUBMOD_RESULT EQUAL "0")
            message(FATAL_ERROR "git submodule update --init failed with ${GIT_SUBMOD_RESULT}, please checkout submodules")
        endif()
    endif()
endif()

if(NOT EXISTS "${PROJECT_SOURCE_DIR}/vendor/GLFW/CMakeLists.txt")
    message(FATAL_ERROR "The submodules were not downloaded! GIT_SUBMODULE was turned off or failed. Please update submodules and try again.")
endif()

if(NOT EXISTS "${PROJECT_SOURCE_DIR}/vendor/ASSIMP/CMakeLists.txt")
    message(FATAL_ERROR "The submodules were not downloaded! GIT_SUBMODULE was turned off or failed. Please update submodules and try again.")
endif()

if(NOT EXISTS "${PROJECT_SOURCE_DIR}/vendor/GLM/CMakeLists.txt")
    message(FATAL_ERROR "The submodules were not downloaded! GIT_SUBMODULE was turned off or failed. Please update submodules and try again.")
endif()
# add_subdirectory(vendor/GLFW)

# include_directories(
# 	"${CMAKE_SOURCE_DIR}/vendor/Glad"
# 	"${CMAKE_SOURCE_DIR}/vendor/GLFW"
# )

# file(GLOB Headers CONFIGURE_DEPENDS "Engine/src/*.h")
# file(GLOB Sources CONFIGURE_DEPENDS "Engine/src/*.cpp")

# add_library(${PROJECT_NAME}_lib STATIC ${Sources} ${Headers})

# add_executable(${PROJECT_NAME} ${Sources} ${Headers})

# target_link_libraries(${PROJECT_NAME}
#     Shell32.lib
#     opengl32.lib
# )