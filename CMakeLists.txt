cmake_minimum_required(VERSION 3.21)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

project(zBot VERSION 3.0.1 LANGUAGES CXX)

file(GLOB SOURCES
    src/framecounting.cpp
    src/gui.cpp
    src/keybinds.cpp
    src/physicsbypass.cpp
    src/playbackmanager.cpp
    src/recordmanager.cpp
    src/speedhackaudio.cpp
    src/clickbetweenframescompat.cpp
    src/zClickbot/clickbot.cpp
    src/zKeyChecker/keychecker.cpp 
)

# 1. Create the library target
add_library(${PROJECT_NAME} SHARED ${SOURCES})

# 2. Add include directories
target_include_directories(${PROJECT_NAME} PRIVATE src)

# 3. Add the required definition to the target AFTER it is created
target_compile_definitions(${PROJECT_NAME} PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)

if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK!")
else()
    message(STATUS "Found Geode: $ENV{GEODE_SDK}")
endif()

add_subdirectory($ENV{GEODE_SDK} $ENV{GEODE_SDK}/build)

CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

target_compile_definitions(${PROJECT_NAME} PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

if (WIN32)
    target_link_libraries(${PROJECT_NAME} PRIVATE imgui-cocos libGDR)
elseif (APPLE)
    target_link_libraries(${PROJECT_NAME} PRIVATE imgui-cocos libGDR "-framework IOKit")
endif()

# 4. Final setup
setup_geode_mod(${PROJECT_NAME})
