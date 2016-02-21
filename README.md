# flatbuffers-compiler
Maven artifact containing compiled binaries for the flatbuffers-compiler (flatc, etc.) for the linux platform.

## How to build the binaries that are placed in this artifact
```
git clone https://github.com/google/flatbuffers.git
cd flatbuffers/build
cmake ..
make
mkdir bin
cp flat* bin
cp libflat* bin
```
The `flatbuffers/build/bin` directory now contains the binaries that are placed in the bin directory of this artifact.

