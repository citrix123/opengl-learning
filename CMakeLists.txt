cmake_minimum_required(VERSION 2.6)
  
project(OPENGLEXAMPLES)

option(BUILD_OGL43 "Bild OpenGL 4.3 examples" OFF)
mark_as_advanced(BUILD_OGL43)

IF(WIN32)
    set("OpenCV_DIR" "C:/Users/rahjain/Downloads/opencv/build/x64/vc14/lib")
    set("OpenCV_Build_DIR" "C:/Users/rahjain/Downloads/opencv/build")
    include_directories(${OpenCV_Build_DIR}/include)
ENDIF()

find_package(OpenGL REQUIRED)
find_package(OpenCV REQUIRED)

add_subdirectory(glfw)
add_subdirectory(glxw)

include_directories(glfw/include)
include_directories(glm)
include_directories(${CMAKE_BINARY_DIR}/glxw/include)

# Set default compile flags for GCC
if(CMAKE_COMPILER_IS_GNUCXX)
    message(STATUS "GCC detected, adding compile flags")
    set(CMAKE_CXX_FLAGS "-O2 -Wall -Wextra")
endif(CMAKE_COMPILER_IS_GNUCXX)

SET(LIBRARIES glfw glxw ${GLFW_LIBRARIES} ${GLXW_LIBRARY} ${OPENGL_LIBRARY} ${OpenCV_LIBS} ${CMAKE_DL_LIBS})

link_directories (${OPENGLEXAMPLES_BINARY_DIR}/bin)
 
add_executable (00skeleton 00skeleton.cpp)
target_link_libraries(00skeleton ${LIBRARIES} )

if(WIN32)
    # Copy DLL files to output directory
    add_custom_command(TARGET 00skeleton POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    "${OpenCV_Build_DIR}/x64/vc14/bin"             
    $<TARGET_FILE_DIR:00skeleton>)
endif() 
