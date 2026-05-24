cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# This handles the Geode SDK discovery automatically
# It looks for the Geode folder that the GitHub Action downloads
if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/geode-sdk")
    add_subdirectory(geode-sdk)
elseif (DEFINED ENV{GEODE_SDK})
    add_subdirectory($ENV{GEODE_SDK})
else()
    message(FATAL_ERROR "Geode SDK not found! Ensure the GitHub Action 'geode-sdk/build-geode-mod' is running correctly.")
endif()

# Create the mod
geode_add_mod(zBot SHARED)

# Add sources
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

# Dependencies & Definitions
target_include_directories(zBot PRIVATE src)
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

target_link_libraries(zBot PRIVATE imgui-cocos libGDR)

# Finalize
setup_geode_mod(zBot)
