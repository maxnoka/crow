#####################################
# Define Project-Wide Settings
#####################################
cmake_minimum_required(VERSION 3.15.0 FATAL_ERROR)

# Define the Project Name and Description
project (crow LANGUAGES CXX)

#####################################
# Define Targets
#####################################
add_library(crow_library INTERFACE)
target_include_directories(crow_library INTERFACE include/)

#####################################
# Misc features
#####################################
macro(get_WIN32_WINNT version)
    if (WIN32 AND CMAKE_SYSTEM_VERSION)
        set(ver ${CMAKE_SYSTEM_VERSION})
        string(REPLACE "." "" ver ${ver})
        string(REGEX REPLACE "([0-9])" "0\\1" ver ${ver})

        set(${version} "0x${ver}")
    endif()
endmacro()

if (MSVC)
    get_WIN32_WINNT(ver)
    target_compile_definitions(crow_library INTERFACE _WIN32_WINNT=${ver})
endif()

#####################################
# Dependencies
#####################################
if (MSVC)
    set(Boost_USE_STATIC_LIBS "On")
    find_package(Boost 1.72.0 REQUIRED COMPONENTS date_time) 
else()
    find_package(Boost 1.72.0) 
endif()

IF (Boost_FOUND)
    message(STATUS "Boost version: ${Boost_VERSION}")
    target_include_directories(crow_library INTERFACE ${Boost_INCLUDE_DIRS})

    if (MSVC)
        target_link_libraries(crow_library INTERFACE Boost::date_time)
    endif()
ELSE()
    message(STATUS "Boost not found")
endif()

find_package(ZLIB REQUIRED)
target_link_libraries(crow_library INTERFACE ZLIB::ZLIB)

set(CMAKE_THREAD_PREFER_PTHREAD ON)
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

target_link_libraries(crow_library INTERFACE Threads::Threads)
