
Building JCuda
----------------------------
 
Most of the JCuda libraries consist of two parts: The native library
which provides the connection to the underlying CUDA library, and the 
Java library that is used for accessing the native library.
 
In order to build all JCuda libraries, create a local working directory,
`C:\JCuda`, and clone all repositories into this directory: 

    git clone https://github.com/jcuda/jcuda-main.git
    git clone https://github.com/jcuda/jcuda-parent.git
    git clone https://github.com/jcuda/jcuda-common.git
    git clone https://github.com/jcuda/jcuda.git
    git clone https://github.com/jcuda/jcublas.git
    git clone https://github.com/jcuda/jcufft.git
    git clone https://github.com/jcuda/jcusparse.git
    git clone https://github.com/jcuda/jcurand.git
    git clone https://github.com/jcuda/jcusolver.git
    git clone https://github.com/jcuda/jnvgraph.git
    git clone https://github.com/jcuda/jcudnn.git

The [jcuda-main](https://github.com/jcuda/jcuda-main) project summarizes
the main JCuda libraries, and contains the top-level build files for
CMake and Maven.

The [jcuda-parent](https://github.com/jcuda/jcuda-parent) project contains
the parent POM for Maven

The [jcuda-common](https://github.com/jcuda/jcuda-common) project contains
build- and source files that are required by all libraries. 

The [jcuda](https://github.com/jcuda/jcuda) project offers the core 
functionality of JCuda, and is used by all other libraries. 

The other projects contain the bindings for the individual CUDA libraries.



## Building the native libraries

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
subdirectory of the respective project, e.g. into 
`C:\JCuda\jcublas\nativeLibraries`.


## Building the Java libraries

The Java libraries can be built with [Apache Maven](https://maven.apache.org/).
After the native libraries have been built as described above, change into 
the `jcuda-parent` directory and execute 

    mvn clean install
    
Then change into the `jcuda-main` directory and execute 

    mvn clean install

This will create the JAR files that contain the native libraries, compile
the Java classes, run the unit tests, assemble the classes, sources and 
JavaDocs into JAR files, and finally place all libraries, together with 
the JAR files that contain the native libraries, into the 
`jcuda-main/output` directory.

---

## A short building script for Linux

This is a set of commands that will build the project, provided you're running on Linux

* OpenJDK 8 installed
* a proper cmake as well

```
RELEASE="version-10.0.0-RC01"
[ -d ./jcuda ] || mkdir ./jcuda
cd jcuda
for project in jcuda-main \
               jcuda-parent \
               jcuda-common \
               jcuda jcublas \
               jcufft \
               jcusparse \
               jcurand \
               jcusolver \
               jnvgraph \
               jcudnn
do
	git clone https://github.com/jcuda/${project}.git
	cd ${project}
	git checkout tags/${RELEASE}
	cd ..
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
