cmake_minimum_required(VERSION 3.21)

# 1. This must be the very first thing you do
include($ENV{GEODE_SDK}/cmake/GeodeConfig.cmake)

# 2. Define the project
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# 3. Create the mod target
geode_add_mod(zBot SHARED)

# 4. Add your source files
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

# 5. Definitions and Includes
target_include_directories(zBot PRIVATE src)
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

# 6. Dependencies
CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

# 7. Finalize
setup_geode_mod(zBot)
