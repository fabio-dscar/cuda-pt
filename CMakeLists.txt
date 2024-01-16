cmake_minimum_required(VERSION 3.20)

project(cuda-pt VERSION 0.1 LANGUAGES CXX C)

enable_language(CUDA)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to 'Release' as none was specified.")
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
    set_property(
        CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo"
    )
endif()

find_package(CUDAToolkit REQUIRED)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/ext)

set(CUDA_PT_SOURCES)

add_executable(cuda-pt src/main.cpp ${CUDA_PT_SOURCES})
target_compile_options(
    cuda-pt PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:-march=native --expt-relaxed-constexpr>
)
target_compile_features(cuda-pt PUBLIC cxx_std_20)
target_compile_definitions(cuda-pt PUBLIC "$<$<CONFIG:Debug>:DEBUG>")
target_include_directories(cuda-pt PUBLIC src ext ${CUDAToolkit_INCLUDE_DIRS})
target_link_libraries(cuda-pt PRIVATE ${OPENGL_LIBRARIES} ${CUDA_cudart_static_LIBRARY})

set(DEBUG_FLAGS -Wall -Wextra -Wpedantic)
set(RELEASE_FLAGS -O3 -march=native)
target_compile_options(
    cuda-pt PRIVATE "$<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:${RELEASE_FLAGS}>"
                    "$<$<CONFIG:Debug>:${DEBUG_FLAGS}>"
)
