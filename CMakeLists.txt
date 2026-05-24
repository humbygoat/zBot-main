cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# Bypass find_package() by manually including the SDK
# The action provides the path in the GEODE_SDK environment variable
if (DEFINED ENV{GEODE_SDK})
    message(STATUS "Loading Geode SDK from $ENV{GEODE_SDK}")
    include("$ENV{GEODE_SDK}/cmake/GeodeConfig.cmake")
else()
    message(FATAL_ERROR "GEODE_SDK environment variable is missing. Check your workflow!")
endif()

# Now that the config is included, these commands will work
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
