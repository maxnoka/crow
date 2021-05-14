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
# Dependencies
#####################################
find_package(Boost 1.5)
IF (Boost_FOUND)
    message(STATUS "Boost version: ${Boost_VERSION}")
    target_include_directories(crow_library INTERFACE ${Boost_INCLUDE_DIRS})
ELSE()
    message(STATUS "Boost not found")
endif()
