cmake_minimum_required(VERSION 3.21)
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# --- THE BULLETPROOF GEODE PATH RESOLVER ---
# Since the GitHub runner environment variables aren't behaving, 
# we force CMake to execute the Geode CLI and ask it exactly where the SDK is installed.
execute_process(
    COMMAND geode sdk path
    OUTPUT_VARIABLE GEODE_CLI_PATH
    OUTPUT_STRIP_TRAILING_WHITESPACE
    ERROR_QUIET
)

if (GEODE_CLI_PATH)
    message(STATUS "Geode CLI reported SDK path: ${GEODE_CLI_PATH}")
    list(APPEND CMAKE_PREFIX_PATH "${GEODE_CLI_PATH}")
    set(Geode_DIR "${GEODE_CLI_PATH}/share/Geode/cmake" CACHE PATH "Force Geode path" FORCE)
endif()
# -------------------------------------------

find_package(Geode REQUIRED)

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
