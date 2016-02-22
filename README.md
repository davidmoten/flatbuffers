# flatbuffers
[![Travis CI](https://travis-ci.org/davidmoten/flatbuffers.svg)](https://travis-ci.org/davidmoten/flatbuffers)<br/>
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.davidmoten/flatbuffers/badge.svg?style=flat)](https://maven-badges.herokuapp.com/maven-central/com.github.davidmoten/flatbuffers)<br/>

Maven artifacts for use with [flatbuffers](https://github.com/google/flatbuffers).

Surprisingly the flatbuffers project team do not publish artifacts of any sort for flatbuffers. Users are expected to build from source. This project shortcuts these actions for you and allows you to do all using Maven artifacts.

Status: *pre-alpha*

##flatbuffers-compiler
Contains compiled binaries for linux platform (and others if someone contributes) to generate java classes (and other) from flatbuffers schema files.

This artifact is used with *maven-dependency-plugin* to unpack, *maven-exec-plugin* to execute and *build-helper-maven-plugin* to add the generated source to the build path. See [example project](example) for details.

##flatbuffers-java
Runtime library artifact for use with flatbuffers generated java classes.

##Example usage
See [example project](example) which is also used to unit test the artifacts.

## How to build the binaries that are placed in this artifact
You don't need to do this (it's why this artifact exists!)
```bash
sudo apt-get install cmake
git clone https://github.com/google/flatbuffers.git
cd flatbuffers/build
cmake ..
make
mkdir bin
cp flat* bin
cp libflat* bin
```
The `flatbuffers/build/bin` directory now contains the binaries that are placed in the bin directory of this artifact.

To run:
```bash
cd src/fbs
../../bin/flatc --java --gen-mutable -o ../../target/generated-sources/java monster.fbs
```

