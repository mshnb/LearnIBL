cmake_minimum_required(VERSION 3.5)

project(LearnIBL)

option(ASSIMP_BUILD_ASSIMP_TOOLS OFF)
option(ASSIMP_BUILD_SAMPLES OFF)
option(ASSIMP_BUILD_TESTS OFF)
add_subdirectory(modules/assimp)

option(GLFW_BUILD_DOCS OFF)
option(GLFW_BUILD_EXAMPLES OFF)
option(GLFW_BUILD_TESTS OFF)
add_subdirectory(modules/glfw)

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -std=c++11")
    if(NOT WIN32)
        set(GLAD_LIBRARIES dl)
    endif()
endif()

include_directories(include/
                    modules/assimp/include/
                    modules/glad/include/
                    modules/glfw/include/
                    modules/glm/)

file(GLOB VENDORS_SOURCES modules/glad/src/glad.c)
file(GLOB PROJECT_HEADERS include/*.h)
file(GLOB PROJECT_SOURCES src/*.cpp)
file(GLOB PROJECT_SHADERS shaders/*.vs
						  shaders/*.fs)

file(GLOB PROJECT_CONFIGS CMakeLists.txt
						  Readme.md
						  .gitignore
						  .gitmodules)

source_group("include" FILES ${PROJECT_HEADERS})
source_group("shaders" FILES ${PROJECT_SHADERS})
source_group("sources" FILES ${PROJECT_SOURCES})
source_group("modules" FILES ${VENDORS_SOURCES})

add_definitions(-DGLFW_INCLUDE_NONE
                -DPROJECT_SOURCE_DIR=\"${PROJECT_SOURCE_DIR}\")
add_executable(${PROJECT_NAME} ${VENDORS_SOURCES} ${PROJECT_HEADERS}
							   ${PROJECT_SOURCES} ${PROJECT_SHADERS} 
							   ${PROJECT_CONFIGS})

target_link_libraries(${PROJECT_NAME} assimp glfw
                      ${GLFW_LIBRARIES} ${GLAD_LIBRARIES})
set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${PROJECT_NAME})

add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_SOURCE_DIR}/shaders $<TARGET_FILE_DIR:${PROJECT_NAME}>
    DEPENDS ${PROJECT_SHADERS})