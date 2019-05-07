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
    set(FIPS_EXCEPTIONS ON)
    set(FIPS_RTTI ON)
    include_directories(libccd/src)
    fips_src(libccd/src RECURSE GROUP)
    add_definitions(-DCCD_DOUBLE)
    fips_end_lib()


    fips_begin_lib(fcl)
    set(FIPS_EXCEPTIONS ON)
    set(FIPS_RTTI ON)
    include_directories(fcl/include)
    fips_src(fcl/src RECURSE GROUP)
    add_definitions(-DCCD_DOUBLE)
    fips_end_lib()

endif()

if (NOT FIPS_IMPORT)
    fips_finish()
endif()