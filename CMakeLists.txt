cmake_minimum_required(VERSION 3.1)

include(${CMAKE_CURRENT_SOURCE_DIR}/../jcuda-common/JCudaCommon_CMake_flags.txt)

project(JCudaMain)

include(${CMAKE_CURRENT_SOURCE_DIR}/../jcuda-common/JCudaCommon_CMake.txt)

add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCuda/JCudaDriverJNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCuda/JCudaDriverJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCuda/JCudaRuntimeJNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCuda/JCudaRuntimeJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCublas/JCublasJNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCublas/JCublasJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCublas/JCublas2JNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCublas/JCublas2JNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCufft/JCufftJNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCufft/JCufftJNI/bin)
    
add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCurand/JCurandJNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCurand/JCurandJNI/bin)

add_subdirectory(
    ${CMAKE_CURRENT_SOURCE_DIR}/../JCusparse/JCusparseJNI
    ${CMAKE_CURRENT_BINARY_DIR}/JCusparse/JCusparseJNI/bin)
    
if(JCUDA_CUDA_VERSION VERSION_GREATER "6.5")    
    add_subdirectory(
        ${CMAKE_CURRENT_SOURCE_DIR}/../JCusolver/JCusolverJNI
        ${CMAKE_CURRENT_BINARY_DIR}/JCusolver/JCusolverJNI/bin)
endif()    

