# flatbuffers-compiler
Maven artifact containing compiled binaries for the flatbuffers-compiler (flatc, etc.) for the linux platform.

##Usage
See the profile with id *test* in [pom.xml](pom.xml) for an example of how to generate java source using the binaries in this artifact.

##Test
```
# build binary tar.gz artifact
mvn clean install
# unpack binary tar.gz artifact and generate sources from it
mvn clean install -Ptest
``` 

## How to build the binaries that are placed in this artifact
You don't need to do this (it's why this artifact exists!)
```bash
## pre-requesite
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

