# flatbuffers
[![Travis CI](https://travis-ci.org/davidmoten/flatbuffers.svg)](https://travis-ci.org/davidmoten/flatbuffers)<br/>
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.davidmoten/flatbuffers-parent/badge.svg?style=flat)](https://maven-badges.herokuapp.com/maven-central/com.github.davidmoten/flatbuffers-parent)<br/>

Maven artifacts for use with [flatbuffers](https://github.com/google/flatbuffers).

The Google flatbuffers project team do not publish artifacts of any sort for flatbuffers to repositories like Maven Central. Users are expected to build from source. This project shortcuts these actions for you and allows you to do all using Maven artifacts from Maven Central.

* Supports flatbuffers 1.3, 1.4, 1.5, 1.6
* Supports Java 1.6+.

Status: *released to Maven Central*

##Versioning
The artifacts carry versions like 1.6.0.1 which correspond to a 0.1 release of the flatbuffers [1.6.0 release](https://github.com/google/flatbuffers/releases/tag/v1.6.0) (from google).

Current versions:

| flatbuffers-compiler, flatbuffers-java | Supports |
| ----------------------------------------- | -------- |
| 1.3.0.1                                   | 1.3      |
| 1.4.0.1                                   | 1.4      |
| 1.5.0.3                                   | 1.5      |
| 1.6.0.2                                   | 1.6      |


##flatbuffers-compiler
Contains compiled binaries to generate java classes (and other) from flatbuffers schema files: 

* linux (x86 64-bit)  (*tar.gz* artifact)
* windows (*zip* artifact)

This artifact is used with *maven-dependency-plugin* to unpack, *maven-exec-plugin* to execute and *build-helper-maven-plugin* to add the generated source to the build path. See [example project](example) for details.

##flatbuffers-java
Runtime library artifact for use with flatbuffers generated java classes.

```xml
<dependency>
    <groupId>com.github.davidmoten</groupId>
    <artifactId>flatbuffers-java</artifactId>
    <version>1.6.0.2</version>
</dependency>
```

##Example usage
See [example project](example) which is also used to unit test the artifacts.

Essentially you add this block of xml to the build/plugins section of your pom.xml to generate flatbuffers classes (java):

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-dependency-plugin</artifactId>
    <version>2.10</version>
    <executions>
        <execution>
            <id>unpack</id>
            <phase>generate-sources</phase>
            <goals>
                <goal>unpack</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>com.github.davidmoten</groupId>
                        <artifactId>flatbuffers-compiler</artifactId>
                        <version>1.6.0.2</version>
                        <type>tar.gz</type>
                        <classifier>distribution-linux</classifier>
                        <overWrite>true</overWrite>
                        <outputDirectory>${project.build.directory}</outputDirectory>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
</plugin>
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
        <execution>
            <goals>
                <goal>exec</goal>
            </goals>
            <phase>generate-sources</phase>
        </execution>
    </executions>
    <configuration>
        <executable>${project.build.directory}/bin/flatc</executable>
        <workingDirectory>${fbs.sources}</workingDirectory>
        <arguments>
            <argument>--java</argument>
            <argument>--gen-mutable</argument>
            <argument>-o</argument>
            <argument>${fbs.generated.sources}</argument>
            <argument>monster.fbs</argument>
        </arguments>
    </configuration>
</plugin>
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>build-helper-maven-plugin</artifactId>
    <version>1.10</version>
    <executions>
        <execution>
            <id>add-source</id>
            <phase>generate-sources</phase>
            <goals>
                <goal>add-source</goal>
            </goals>
            <configuration>
                <sources>
                    <source>${fbs.generated.sources}</source>
                </sources>
            </configuration>
        </execution>
    </executions>
</plugin>
```

There are a couple of properties mentioned in the xml block above. I set them to these values for my projects:

```xml
<properties>
    <fbs.sources>${basedir}/src/main/fbs</fbs.sources>
    <fbs.generated.sources>${project.build.directory}/generated-sources/java</fbs.generated.sources>
</properties>
```

To use the generated classes you'll need the runtime dependency *flatbuffers-java*. Add the following to the dependencies section in pom.xml:

```xml
<dependency>
    <groupId>com.github.davidmoten</groupId>
    <artifactId>flatbuffers-java</artifactId>
    <version>1.6.0.1</version>
</dependency>
```

## How to build the binaries that are placed in this artifact
You don't need to do this (it's why this artifact exists!):

```bash
sudo apt-get install cmake
git clone https://github.com/google/flatbuffers.git
git checkout v1.5.0
cd flatbuffers
mkdir build
cd build
cmake ..
make
mkdir bin
cp flat* bin
cp libflat* bin
```
The `flatbuffers/build/bin` directory now contains the binaries that are placed in the bin directory of this artifact.

In addition, the java source classes need to be copied from `flatbuffers/java`(google repo) into `flatbuffers/flatbuffers-java/src/main/java`.

To generate java sources manually here's an example:
```bash
cd src/fbs
../../bin/flatc --java --gen-mutable -o ../../target/generated-sources/java monster.fbs
```

###For project maintainers
To update this project with executables for a new version of flatbuffers: 
* copy the files in `flatbuffers/build/bin` above to `flatbuffers-compiler/bin/linux/`
* download `flatc_windows_exe.zip` from [releases](https://github.com/google/flatbuffers/releases), unpack it and copy `flatc.exe` to `flatbuffers-compiler/bin/windows/`
* build and release!


