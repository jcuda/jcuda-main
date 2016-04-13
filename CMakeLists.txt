cmake_minimum_required(VERSION 3.1)

include(${CMAKE_CURRENT_SOURCE_DIR}/../jcuda-common/JCudaCommon_CMake_flags.txt)

project(JCudaMain)

include(${CMAKE_CURRENT_SOURCE_DIR}/../jcuda-common/JCudaCommon_CMake.txt)

add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcuda/JCudaDriverJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcuda/JCudaDriverJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcuda/JCudaRuntimeJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcuda/JCudaRuntimeJNI/bin)

add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jnvrtc/JNvrtcJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jnvrtc/JNvrtcJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcublas/JCublasJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcublas/JCublasJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcublas/JCublas2JNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcublas/JCublas2JNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcufft/JCufftJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcufft/JCufftJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcurand/JCurandJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcurand/JCurandJNI/bin)

add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../jcusparse/JCusparseJNI
    ${CMAKE_CURRENT_BINARY_DIR}/jcusparse/JCusparseJNI/bin)
    
if(JCUDA_CUDA_VERSION VERSION_GREATER "6.5")    
    add_subdirectory(
        ${CMAKE_CURRENT_SOURCE_DIR}/../jcusolver/JCusolverJNI
        ${CMAKE_CURRENT_BINARY_DIR}/jcusolver/JCusolverJNI/bin)
endif()    

