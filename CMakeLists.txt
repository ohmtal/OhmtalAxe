cmake_minimum_required(VERSION 3.16)
project(IncludeTestProject LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# OhmFlux flags:
set(DISABLE_DEMOS ON CACHE BOOL "Disable engine demos" FORCE)
set(WITH_BOX2D OFF CACHE BOOL "Enable Box 2D : off" FORCE)

set(OHMFLUX_DIR "/opt/OhmFlux" CACHE PATH "Path to the OhmFlux engine source directory")


include(FetchContent)

if( ("${OHMFLUX_DIR}" STREQUAL "") OR (NOT EXISTS "${OHMFLUX_DIR}/CMakeLists.txt"))
    message(WARNING "The directory OHMFLUX_DIR='${OHMFLUX_DIR}' does not contain a CMakeLists.txt file! Fetching from github.")

    FetchContent_Declare(
        ohmFlux
        GIT_REPOSITORY https://github.com/ohmtal/OhmFlux
        GIT_TAG        main
    )
    FetchContent_MakeAvailable(ohmFlux)

    set(OHMFLUX_DIR ${ohmFlux_SOURCE_DIR})

else()
    add_subdirectory("${OHMFLUX_DIR}" "${CMAKE_BINARY_DIR}/ohmFlux_build")
endif()




# --- Ohmtal Axe ---
option(BUILD_AXE "Build Ohmtal Axe application" ON)
if(BUILD_AXE)
    set(AXE_DIR "${CMAKE_CURRENT_LIST_DIR}/OhmtalAxe")
    set(APP_SOURCES
        "${AXE_DIR}/src/main.cpp"
        "${AXE_DIR}/src/appGui.cpp"
        "${AXE_DIR}/src/modules/soundMixModule.cpp"
        "${AXE_DIR}/src/modules/inputModule.cpp"
        "${AXE_DIR}/src/modules/rackModule.cpp"
        "${AXE_DIR}/src/modules/keyboardModule.cpp"
        "${AXE_DIR}/src/modules/drumKitLooper.cpp"
    )
    configure_project_target(OhmtalAxe ${AXE_DIR} "${AXE_DIR}/assets@assets" "${APP_SOURCES}")
endif()
