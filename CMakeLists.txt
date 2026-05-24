cmake_minimum_required(VERSION 3.21)

# 1. Project setup
project(zBot VERSION 3.0.1 LANGUAGES CXX)

# 2. Find and include Geode SDK
if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable.")
endif()
add_subdirectory($ENV{GEODE_SDK} $ENV{GEODE_SDK}/build)

# 3. Define the mod (This replaces add_library)
geode_add_mod(zBot)

# 4. Add your sources
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

# 5. Add include directories
target_include_directories(zBot PRIVATE src)

# 6. Compile definitions (Fix for your keyboard dispatcher error)
target_compile_definitions(zBot PRIVATE CC_KEYBOARD_DISPATCHER_FOUR_ARGS)
target_compile_definitions(zBot PRIVATE MOD_VERSION="${CMAKE_PROJECT_VERSION}")

# 7. Dependencies
CPMAddPackage("gh:maxnut/GDReplayFormat#9c32b5529067901418ea6e4dfbd36437dc8712bc")
CPMAddPackage("gh:matcool/gd-imgui-cocos#8973953519fa05d81c61634e585559ab87ca559d")

# 8. Linking (Standardized to use PRIVATE to prevent conflicts)
target_link_libraries(zBot PRIVATE imgui-cocos libGDR)
if (APPLE)
    target_link_libraries(zBot PRIVATE "-framework IOKit")
endif()

# 9. Final Geode setup
setup_geode_mod(zBot)
