set(FIPS_EXCEPTIONS ON)
set(FIPS_RTTI ON)


if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-fcl)
endif()


if ((FIPS_MACOS OR FIPS_WINDOWS OR FIPS_LINUX) AND NOT (FIPS_RASPBERRYPI OR FIPS_OSX_USE_ARC OR FIPS_UWP))




    fips_begin_lib(libccd)

    if (MSVC)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP /bigobj") ### Enable parallel compilation
        string(REPLACE "/WX" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
        string(REPLACE "/WX" "" CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
        add_definitions(-DCCD_STATIC_DEFINE)
    endif(MSVC)


    set(FIPS_EXCEPTIONS ON)
    set(FIPS_RTTI ON)
    include_directories(libccd/src)
    fips_src(libccd/src RECURSE GROUP)
    fips_end_lib()


    fips_begin_lib(fcl)

    if (MSVC)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP /bigobj") ### Enable parallel compilation
        string(REPLACE "/WX" "" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
        string(REPLACE "/WX" "" CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
        add_definitions(-DCCD_STATIC_DEFINE)
    endif(MSVC)
    
    set(FIPS_EXCEPTIONS ON)
    set(FIPS_RTTI ON)
    include_directories(fcl/include)
    fips_src(fcl/src RECURSE GROUP)
    fips_end_lib()

endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()