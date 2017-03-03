cmake_minimum_required(VERSION 2.6)
project(struck)

if (NOT CMAKE_BUILD_TYPE)
    message(STATUS "No build type selected, defaulting to Release")
    set(CMAKE_BUILD_TYPE "Release")
endif()

set(CMAKE_MODULE_PATH ${CMAKE_HOME_DIRECTORY}/cmake ${CMAKE_MODULE_PATH})
message(STATUS ${CMAKE_HOME_DIRECTORY})

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)


#find_package(Eigen REQUIRED HINTS ${CMAKE_HOME_DIRECTORY})
#find_package(OpenCV REQUIRED)

# add OpenCV
set(OPENCV_DIR_HINT "")

if(WIN32)
    get_filename_component(OPENCV_DIR_PLATFORM $ENV{OPENCV_DIR} DIRECTORY)
    get_filename_component(OPENCV_DIR_HINT ${OPENCV_DIR_PLATFORM} DIRECTORY)
endif(WIN32)

set(OpenCV_STATIC OFF)
find_package(OpenCV REQUIRED HINTS ${OPENCV_DIR_HINT})

SET( EIGEN_INCLUDE_DIRS ${CMAKE_HOME_DIRECTORY} )

include_directories(
    src
    ${EIGEN_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
)

# add all .cpp and .h files in src
file(GLOB_RECURSE SRC RELATIVE ${PROJECT_SOURCE_DIR} src/*.cpp)
file(GLOB_RECURSE HEADERS RELATIVE ${PROJECT_SOURCE_DIR} src/*.h)

add_executable(struck ${HEADERS} ${SRC})

target_link_libraries(struck
    ${OpenCV_LIBS}
)
