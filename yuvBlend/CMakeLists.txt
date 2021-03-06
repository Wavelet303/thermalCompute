cmake_minimum_required(VERSION 2.8.7)
project(YUVBlender)

# add c++11 support
if(CMAKE_COMPILER_IS_GNUCC)
    ADD_DEFINITIONS ( -std=c++11 )
endif(CMAKE_COMPILER_IS_GNUCC)

# add OpenCV lib
find_package(OpenCV REQUIRED)
message(STATUS "OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

set(BLENDER_DIR "src")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

include_directories(${OpenCV_DIR}/include
		    ${PROJECT_SOURCE_DIR}/include
)

#compile as shared library
add_library(${PROJECT_NAME} SHARED
    ${BLENDER_DIR}/src/edge_image_blender.cpp
    ${BLENDER_DIR}/src/edge_image_scale_blender.cpp
    ${BLENDER_DIR}/src/yuneec_cv_utilities.cpp
    ${BLENDER_DIR}/src/edge_extractor.cpp
    ${BLENDER_DIR}/src/canny_edge_extractor.cpp
    ${BLENDER_DIR}/src/dog_edge_extractor.cpp
    ${BLENDER_DIR}/src/laplacian_edge_extractor.cpp
    ${BLENDER_DIR}/src/sobel_edge_extractor.cpp
    ${BLENDER_DIR}/src/blender_input.cpp
    ${BLENDER_DIR}/src/utils.cpp
)

target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
)

# add executable
add_executable(blender
    ${BLENDER_DIR}/main.cpp
)
    
target_link_libraries(blender ${OpenCV_LIBS} ${PROJECT_NAME})



