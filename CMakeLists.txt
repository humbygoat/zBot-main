cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# Find Geode package (this is the standard way)
find_package(Geode REQUIRED)

# Use geode_add_mod (it will work now because find_package loaded it)
geode_add_mod(zBot SHARED)

# Add your sources
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

# Compile definitions
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

# Dependencies
CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

target_link_libraries(zBot PRIVATE imgui-cocos libGDR)

# Final Geode setup
setup_geode_mod(zBot)
