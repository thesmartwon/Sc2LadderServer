cmake_minimum_required(VERSION 3.1)

project(5minBot)

# Specify output directories.
set(EXECUTABLE_OUTPUT_PATH "${PROJECT_BINARY_DIR}/bin")

# Include custom macros to find SC2Api.
set (CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

find_package(SC2Api REQUIRED)

# Build with c++14 support, required by sc2api.
set(CMAKE_CXX_STANDARD 14)

# All the source files for the bot.
file(GLOB BOT_SOURCES "*.cpp" "*.h" "AutoObserver/*.cpp" "AutoObserver/*.h")

include_directories(SYSTEM "${SC2Api_INCLUDE_DIRS}")

# Show more warnings at compiletime.
if (MSVC)
    # FIXME: put Windows specific options here.
else ()
    set(CMAKE_CXX_FLAGS "-Wall -Wextra")
endif ()

# Create the executable.
add_executable(5minBot ${BOT_SOURCES})
target_link_libraries(5minBot ${SC2Api_LIBRARIES})

if (APPLE)
    target_link_libraries(5minBot "-framework Carbon")
endif ()

# Linux specific.
if (UNIX AND NOT APPLE)
    target_link_libraries(5minBot pthread dl)
endif ()
