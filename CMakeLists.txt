cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# Do NOT call add_subdirectory($ENV{GEODE_SDK}...) 
# The build-geode-mod action handles this for you.

# 1. Define the mod
geode_add_mod(zBot SHARED)

# 2. Sources
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

# 3. Include directories
target_include_directories(zBot PRIVATE src)

# 4. Compile definitions
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

# 5. Dependencies
CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

# 6. Linking
target_link_libraries(zBot PRIVATE imgui-cocos libGDR)

# 7. Final Geode setup
setup_geode_mod(zBot)
