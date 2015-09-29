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

Each of these projects contains the source code for the native libraries
(DLL, SO, or DYLIB) and for the Java libraries (JAR). 

The [jcuda-common](https://github.com/jcuda/jcuda-common) project contains
additional build- and source files that are required by all libraries.


Pre-built binaries and Maven support 
------------------

Pre-built binaries are available at the 
[JCuda downloads page](http://www.jcuda.org/downloads/downloads.html).

Konstantin Perikov is maintaining 
[Mavenized JCuda](https://github.com/MysterionRise/mavenized-jcuda),
which offers the JCuda binaries as Maven artifacts.  



Build instructions
------------------

The core JCuda libraries (JCuda, JCublas, JCufft, JCurand and JCusolver)
are summarized in this `jcuda-main` project. It contains build files that
refer to the individual sub-projects. When these projects and the
[jcuda-common](https://github.com/jcuda/jcuda-common) project are located 
in a local working directory, the build process is as follows:

* Run [CMake](http://www.cmake.org/), using the `CMakeLists.txt` from
the `jcuda-main` project as an input. Generate the build files in an
arbitrary output directory (for example, `jcuda-main-build`). Compiling
these build files with the chosen native compiler will cause the native
libraries (DLL, SO or DYLIB files) to be placed in a subdirectory of 
the working directory, called `nativeLibraries`.

* Run [Apache Maven](https://maven.apache.org/) using the `pom.xml` from
the `jcuda-main` project as an input. Invoking `mvn clean package` will 
compile the Java libraries, run the unit tests, assemble the binaries,
sources and JavaDocs into JAR files, and finally place all libraries
into the `jcuda-main/target` directory.




