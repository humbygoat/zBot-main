cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# 1. Look for Geode explicitly
if (NOT DEFINED ENV{GEODE_SDK})
    # If the environment variable isn't set, look in the common GitHub action path
    set(ENV{GEODE_SDK} "${CMAKE_CURRENT_SOURCE_DIR}/geode-sdk-clone")
endif()

message(STATUS "Using Geode SDK at: $ENV{GEODE_SDK}")
add_subdirectory("$ENV{GEODE_SDK}" "${CMAKE_BINARY_DIR}/geode")

# 2. Now that the SDK is added, Geode commands SHOULD work.
# If geode_add_mod still fails, it means the SDK folder is empty or corrupted.
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

if (APPLE)
    target_link_libraries(zBot PRIVATE "-framework IOKit")
endif()

setup_geode_mod(zBot)
