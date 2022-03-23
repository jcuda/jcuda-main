
Using JCuda
------------------

This page explains how to use JCuda inside your project, depending on
whether you are using Maven, Gradle or a manual build process.

For a general introduction on how to use the runtime libraries and
how to create kernels, refer to the [JCuda Tutorial website](http://www.jcuda.org/tutorial/TutorialIndex.html)

- [Using JCuda as a Maven dependency](#using-jcuda-with-maven)
- [Using JCuda with Gradle](#using-jcuda-with-gradle)
- [Using the JCuda libraries manually](#using-jcuda-manually)


# Using JCuda with Maven

Since version 0.8.0, JCuda is available in Maven Central. In order to use
JCuda in your Maven project, just add the necessary dependencies to your
`pom.xml`. The list of all available artifacts is shown here:

    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcuda</artifactId>
        <version>11.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcublas</artifactId>
        <version>11.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcufft</artifactId>
        <version>11.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcusparse</artifactId>
        <version>11.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcusolver</artifactId>
        <version>11.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcurand</artifactId>
        <version>11.5.2</version>
    </dependency>
    <dependency>
        <groupId>org.jcuda</groupId>
        <artifactId>jcudnn</artifactId>
        <version>11.5.2</version>
    </dependency>

The `jcuda` dependency contains the core library and is required
in any case. Beyond that, only the necessary dependencies have
to be added. For example, in order to use JCublas, the bindings 
for CUBLAS, only the `jcublas` dependency is required. 

Each of these dependencies refers to a JAR file. And each of
these JAR files defines further dependencies to other JAR files
that contain the native libraries. Maven should automatically
detect these dependencies, and download the required JAR files,
depending on the operating system.

For earlier versions of JCuda, Konstantin Perikov is maintaining 
[Mavenized JCuda](https://github.com/MysterionRise/mavenized-jcuda),
which offers the JCuda binaries as Maven artifacts.  


# Using JCuda with Gradle

In order to use JCuda with Gradle, it is necessary to declare
the dependencies to the JAR files that contain the native libraries.
These dependencies will be different for each operating system.

Here is a template for a Gradle build file that detects the 
required dependencies. 

(This template is based on the 
[discussion in an issue report](https://github.com/jcuda/jcuda-main/issues/16#issuecomment-323610823). 
Thanks to [evbarnett](https://github.com/evbarnett) and 
[blvp](https://github.com/blvp) for setting this up)


    apply plugin: 'java'

    // Methods to determine the operating system (OS) and architecture (Arch) of the system.
    // These strings are used to determine the classifier of the artifact that contains the
    // native libaries. For example, when the operating system is "windows" and the
    // architecture is "x86_64", then the classifier will be "windows-x86_64", and thus,
    // the JAR file containing the native libraries will be
    // jcuda-natives-windows-x86_64-11.0.0.jar
    // These methods are taken from
    // https://github.com/jcuda/jcuda/blob/master/JCudaJava/src/main/java/jcuda/LibUtils.java
    def static getOsString() {
        String vendor = System.getProperty("java.vendor");
        if ("The Android Project" == vendor) {
        return "android";
        } else {
        String osName = System.getProperty("os.name");
        osName = osName.toLowerCase(Locale.ENGLISH);
        if (osName.startsWith("windows")) {
            return "windows";
        } else if (osName.startsWith("mac os")) {
            return "apple";
        } else if (osName.startsWith("linux")) {
            return "linux";
        } else if (osName.startsWith("sun")) {
            return "sun"
        }
        return "unknown"
        }
    }

    def static getArchString() {
        String osArch = System.getProperty("os.arch");
        osArch = osArch.toLowerCase(Locale.ENGLISH);
        if ("i386" == osArch || "x86" == osArch || "i686" == osArch) {
        return "x86";
        } else if (osArch.startsWith("amd64") || osArch.startsWith("x86_64")) {
        return "x86_64";
        } else if (osArch.startsWith("arm64")) {
        return "arm64";
        } else if (osArch.startsWith("arm")) {
        return "arm";
        } else if ("ppc" == osArch || "powerpc" == osArch) {
        return "ppc";
        } else if (osArch.startsWith("ppc")) {
        return "ppc_64";
        } else if (osArch.startsWith("sparc")) {
        return "sparc";
        } else if (osArch.startsWith("mips64")) {
        return "mips64";
        } else if (osArch.startsWith("mips")) {
        return "mips";
        } else if (osArch.contains("risc")) {
        return "risc";
        }
        return "unknown";
    }

    dependencies {
        implementation fileTree(include: ['*.jar'], dir: 'libs')

        // Add your other dependencies here:

        // JCuda dependencies are below

        def classifier = getOsString() + "-" + getArchString()

        // Set JCuda version here, or if multiple modules use JCuda,
        // you should set a global variable like so:
        //
        // ext {
        //  jCudaVersion = "11.0.0"
        // }
        //
        // In your *top level* build gradle, and use
        // rootProject.ext.jCudaVersion instead of jCudaVersion when you need to access it

        def jCudaVersion = "11.0.0"

        // JCuda Java libraries

        implementation(group: 'org.jcuda', name: 'jcuda', version: jCudaVersion) {
        transitive = false
        }
        implementation(group: 'org.jcuda', name: 'jcublas', version: jCudaVersion) {
        transitive = false
        }
        implementation(group: 'org.jcuda', name: 'jcufft', version: jCudaVersion) {
        transitive = false
        }
        implementation(group: 'org.jcuda', name: 'jcusparse', version: jCudaVersion) {
        transitive = false
        }
        implementation(group: 'org.jcuda', name: 'jcurand', version: jCudaVersion) {
        transitive = false
        }
        implementation(group: 'org.jcuda', name: 'jcusolver', version: jCudaVersion) {
        transitive = false
        }
        implementation(group: 'org.jcuda', name: 'jcudnn', version: jCudaVersion) {
        transitive = false
        }

        // JCuda native libraries

        implementation group: 'org.jcuda', name: 'jcuda-natives',
            classifier: classifier, version: jCudaVersion
        implementation group: 'org.jcuda', name: 'jcublas-natives',
            classifier: classifier, version: jCudaVersion
        implementation group: 'org.jcuda', name: 'jcufft-natives',
            classifier: classifier, version: jCudaVersion
        implementation group: 'org.jcuda', name: 'jcusparse-natives',
            classifier: classifier, version: jCudaVersion
        implementation group: 'org.jcuda', name: 'jcurand-natives',
            classifier: classifier, version: jCudaVersion
        implementation group: 'org.jcuda', name: 'jcusolver-natives',
            classifier: classifier, version: jCudaVersion
        implementation group: 'org.jcuda', name: 'jcudnn-natives',
            classifier: classifier, version: jCudaVersion
    }

# Using JCuda manually

In order to use JCuda manually, for example, in an IDE or a custom build process,
the pre-built binaries for each operating system can be downloaded from the 
[JCuda downloads page](http://www.jcuda.org/downloads/downloads.html).

These JAR files usually just have to be added to the `classpath`. JCuda will
then be available, and automatically unpack the required native libraries from
the JAR files at runtime.






