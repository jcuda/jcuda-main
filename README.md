# jcuda-main

Project structure
-----------------

[JCuda](http://jcuda.org/) offers Java bindings for CUDA and CUDA-related 
libraries. The bindings for the individual libraries are contained in the
following sub-projects:

* [JCuda](https://github.com/jcuda/jcuda) -- The main JCuda library, offering
access to the CUDA runtime- and driver API. This library (the JAR file) is
required for compiling and using all other libraries.
* [JCublas](https://github.com/jcuda/jcublas) -- Java bindings for CUBLAS
* [JCufft](https://github.com/jcuda/jcufft) -- Java bindings for CUFFT
* [JCurand](https://github.com/jcuda/jcurand) -- Java bindings for CURAND
* [JCusparse](https://github.com/jcuda/jcusparse) -- Java bindings for CUSPARSE
* [JCusolver](https://github.com/jcuda/jcusolver) -- Java bindings for CUSOLVER
* <strike>[JNvgraph](https://github.com/jcuda/jnvgraph) -- Java bindings for nvGRAPH</strike> Deprecated as of CUDA 11
* [JCudnn](https://github.com/jcuda/jcudnn) -- Java bindings for cuDNN

Each of these projects contains the source code for the native libraries
(DLL, SO, or DYLIB) and for the Java libraries (JAR). 

Using JCuda
------------------

JCuda can be used with Maven, Gradle or by downloading the pre-built libraries. 
Details are explained here: [Using JCuda](USAGE.md)


Building JCuda
------------------

Information about how to build the JCuda libraries from the source code is
given here: [Building JCuda](BUILDING.md)

