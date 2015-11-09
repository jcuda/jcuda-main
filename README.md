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
 
In order to build all JCuda libraries, create a local working directory,
`C:\JCuda`, and clone all repositories into this directory: 

    git clone https://github.com/jcuda/jcuda-common.git
    git clone https://github.com/jcuda/jcuda-main.git
    git clone https://github.com/jcuda/jcuda.git
    git clone https://github.com/jcuda/jcublas.git
    git clone https://github.com/jcuda/jcufft.git
    git clone https://github.com/jcuda/jcusparse.git
    git clone https://github.com/jcuda/jcurand.git
    git clone https://github.com/jcuda/jcusolver.git


**Building the native libraries**

The native libraries of JCuda can be built with [CMake](http://www.cmake.org/)
and any compatible target compiler (e.g. Visual Studio or GCC):

* Start `cmake-gui`,
* Set the directory containing the sources of the `jcuda-main` project, e.g. `C:\JCuda\jcuda-main`
* Set the directory for the build files: e.g. `C:\JCuda.build`
* Press "Configure" (and select the appropriate compiler)
* Press "Generate"

Then, `C:\JCuda.build` will contain the build files, e.g. the
GCC makefiles or the Visual Studio project files. Compiling the
with these makefiles will place the binaries into a `nativeLibraries`
subdirectory of the `jcuda-main` project, e.g. into 
`C:\JCuda\jcuda-main\nativeLibraries`.


**Building the Java libraries**

The actual Java libraries can be built with 
[Apache Maven](https://maven.apache.org/). After the native libraries
have been built into the `jcuda-main\nativeLibraries`
directory as described above, change into the `jcuda-main` directory
and execute 

    mvn clean package

This will compile the Java libraries, run the unit tests, assemble the 
classes, native libraries, sources and JavaDocs into JAR files, and 
finally place all libraries into the `jcuda-main/target` directory.




