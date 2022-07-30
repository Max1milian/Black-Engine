cmake_minimum_required(VERSION 3.23.2 FATAL_ERROR)

#running a 64bit check at launch
if(CMAKE_SIZEOF_VOID_P EQUAL 4)
	message(FATAL_ERROR "Error! 32bit System detected. BlackEngine supports 64bit Architectures only.")
endif(CMAKE_SIZEOF_VOID_P EQUAL 4)

project(BlackEngine
	LANGUAGES CXX
	VERSION 0.0.1
	HOMEPAGE_URL "https://github.com/Max1milian/Black-Engine"
	DESCRIPTION "A 3D game engine made with modern C++")

include(cmake/dependencies.cmake)
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#making the compiler more strict with error handling
if(MSVC)
    add_compile_options(/W3 /WX /permissive)
else()
    add_compile_options(-Wextra -Wall -Werror -pedantic)
endif(MSVC)
add_subdirectory(src)

add_executable(${PROJECT_NAME} src/main.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_include_directories(${PROJECT_NAME} PRIVATE ${LUA_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME} PRIVATE Vulkan::Vulkan OpenAL::OpenAL glad::glad fmt::fmt glm::glm ${LUA_LIBRARIES} EASTL asio glfw)
