cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# The GitHub Action automatically sets the GEODE_SDK environment variable.
# We use that to include the SDK.
if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "GEODE_SDK environment variable not set. Are you using the geode-sdk/build-geode-mod action?")
endif()

add_subdirectory($ENV{GEODE_SDK} $ENV{GEODE_SDK}/build)

# Now that the SDK is loaded via add_subdirectory, 
# the Geode commands (like geode_add_mod) are available.
geode_add_mod(zBot)

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

# Fix for the keyboard dispatcher
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

target_link_libraries(zBot PRIVATE imgui-cocos libGDR)

setup_geode_mod(zBot)
