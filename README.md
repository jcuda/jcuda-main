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
with these makefiles will place the binaries into a `nativeLibrary`
subdirectory of the respective project, e.g. into 
`C:\JCuda\jcublas\JCublasJNI\nativeLibrary`.


**Building the Java libraries**

The actual Java libraries can be built with 
[Apache Maven](https://maven.apache.org/). After the native libraries
have been built as described above, change into the `jcuda-main` directory
and execute 

    mvn clean install

This will copy the native libraries into the target folder for the 
JARs, compile the Java libraries, run the unit tests, assemble the 
classes, sources and JavaDocs into JAR files, and finally place all 
libraries, together with the native libraries, into the 
`jcuda-main/target` directory.


Short building script
---------------------

This is a set of commands that will build the project, provided you're running on Linux

* OpenJDK 8 installed
* a proper cmake as well

```
[ -d ./jcuda ] || mkdir ./jcuda
cd jcuda
for project in jcuda-common jcuda-main jcuda jcublas jcufft jcusparse jcurand jcusolver
do 
	git clone https://github.com/jcuda/${project}.git
	cd ${project}
	git checkout tags/version-0.7.5b # This is optional, only if compiling on CUDA 0.7.5
	cd /mnt/jcuda
done
cmake ./jcuda-main
make all
cd jcuda-main

case "$(arch)" in 
	"x86_64" | "amd64" ) 
		JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64" mvn clean install
	;;
	"ppc64le" | "ppc64el" )
		JAVA_HOME="/usr/lib/jvm/java-8-openjdk-ppc64el" mvn clean install
	;;
	* )
		echo "Your architecture is not supported yet, but you can do this!"
		echo "find your JDK path and type : "
		echo "JAVA_HOME=/path/to/java mvn clean install"
	;;
esac
```

Notes: 

Some of the software pieces needed here are cutting edge, and packaging is not necessarily supported or provided
by the standard distributions. You'll have to build them from source, or use some personal repositories. 
The below examples have been tested at the time of this writing, and are provided as-is, without any 
guarantees. Feel free to add, change, update this based on your own experience. 

* On Ubuntu 16.04, OpenJDK 8 is the default
* On Ubuntu 14.04, do 

``` 
sudo apt-get update -qq
sudo apt-get install software-properties-common
sudo apt-add-repository -y ppa:openjdk-r/ppa
sudo apt-add-repository -y ppa:jochenkemnade/openjdk-8
apt-get update -qq
sudo apt-get install -yqq --force-yes openjdk-8-jdk openjdk-8-jre-headless
```

* On Ubuntu 16.04, cmake is in 3.5.1, OK for this
* On Ubuntu 14.04, cmake is a 2.8.x and won't work. Fix with: 

```
sudo add-apt-repository -y ppa:george-edison55/cmake-3.x
sudo apt-get update -qq && \
sudo apt-get install -yqq cmake
```

* On Ubuntu 16.04, maven is 3.3.9 by default
* On Ubuntu 14.04, if you wish to use maven 3.x, do 

```
sudo apt-get remove maven
sudo apt-add-repository -y ppa:andrei-pozolotin/maven3
sudo apt-get install -qq maven3
```