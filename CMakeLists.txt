cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# The GitHub Action sets this variable. We use it to manually include the SDK.
# This bypasses the need for find_package to "search" the system.
if (DEFINED ENV{GEODE_SDK})
    message(STATUS "Manually including Geode from: $ENV{GEODE_SDK}")
    include("$ENV{GEODE_SDK}/cmake/GeodeConfig.cmake")
else()
    message(FATAL_ERROR "GEODE_SDK is not set! The GitHub Action did not initialize correctly.")
endif()

# Now geode_add_mod will be recognized because GeodeConfig.cmake is included
geode_add_mod(zBot SHARED)

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
