cmake_minimum_required(VERSION 3.21)

# Ensure the SDK path is defined via environment variable
if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "GEODE_SDK environment variable is not set. Is the GitHub Action running correctly?")
endif()

# Set the path to the SDK based on the environment variable provided by the action
list(APPEND CMAKE_PREFIX_PATH "$ENV{GEODE_SDK}")

project(zBot VERSION 3.0.1 LANGUAGES CXX)

# Use the Geode SDK directly
find_package(Geode REQUIRED)

geode_add_mod(zBot SHARED)

# Add your source files
target_sources(zBot PRIVATE
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

target_include_directories(zBot PRIVATE src)
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

target_link_libraries(zBot PRIVATE imgui-cocos libGDR)

setup_geode_mod(zBot)
