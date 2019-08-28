# Building fabric-sdk-java

The instructions provided below specify the steps to build fabric-sdk-java release-1.4 on Linux on Z for the following distributions:

Ubuntu (16.04)

*General Notes:*

- *When following the steps below please use a standard permission user unless otherwise specified.*
- *A directory `/<source_root>/` will be referred to in these instructions. This is a temporary writable directory anwyhwere you'd like to place it.*

## Part 1: Building protobuf

### Step 1: Install dependencies

```bash
export SOURCE_ROOT=/<source_root>/
```

- Ubuntu (16.04)

```bash
sudo apt-get update 
sudo apt-get install tar wget autoconf automake g++ git gzip libtool make bzip2 curl unzip zlib1g-dev openjdk-8-jdk-headless maven ninja-build cmake libssl-dev perl golang libapr1-dev
```

### Step 2: Download and build protobuf

2.1) Clone the GitHub repository

```bash
cd $SOURCE_ROOT
git clone https://github.com/linux-on-ibm-z/protobuf.git
cd $SOURCE_ROOT/protobuf
git checkout devs390x
```

2.2) Initialize any submodules

```bash
git submodule update --init --recursive
```

2.3) Generate the configuration script

```bash
./autogen.sh
```

2.4) Build for s390x

```bash
protoc-artifacts/build-protoc.sh linux s390x_64 protoc
```

### Step 3: Install protobuf

3.1) Add the protoc compiler to the local maven repository

```bash
mvn install:install-file -DgroupId=com.google.protobuf -DartifactId=protoc -Dversion=3.0.0 -Dclassifier=linux-s390_64 -Dpackaging=exe -Dfile=$SOURCE_ROOT/protobuf/protoc-artifacts/target/linux/s390x_64/protoc.exe
```

3.2) Add the include files to the local system

```bash
cd $SOURCE_ROOT/protobuf/protoc-artifacts/build/linux/s390x_64
sudo make install
```


## Part 2: Building grpc-java

### Step 1: Install dependencies

```bash
export SOURCE_ROOT=/<source_root>/
```

- Ubuntu (16.04)

```bash
sudo apt-get update 
sudo apt-get install tar wget autoconf automake g++ git gzip libtool make bzip2 curl unzip zlib1g-dev openjdk-8-jdk-headless maven ninja-build cmake libssl-dev perl golang libapr1-dev
```

### Step 2: Download and build grpc-java

2.1) Clone the GitHub repository

```bash
cd $SOURCE_ROOT
git clone https://github.com/linux-on-ibm-z/grpc-java.git
cd $SOURCE_ROOT/grpc-java
git checkout gRPC_Java_s390x
```
 
2.2) Build for s390x

```bash
cd $SOURCE_ROOT/grpc-java/compiler
../gradlew java_pluginExecutable
```

### Step 3: Install grpc-java

3.1) Add the grpc plugin executable to the local maven repository

```bash
mvn install:install-file -DgroupId=io.grpc -DartifactId=protoc-gen-grpc-java -Dversion=1.17.1 -Dclassifier=linux-s390_64 -Dpackaging=exe -Dfile=$SOURCE_ROOT/grpc-java/compiler/build/exe/java_plugin/protoc-gen-grpc-java

```

## Part 3: Build netty-tcnative

### Step 1: Download and build netty-tcnative

Follow the instructions at [https://github.com/linux-on-ibm-z/docs/wiki/Building-netty-tcnative](https://github.com/linux-on-ibm-z/docs/wiki/Building-netty-tcnative) to compile netty-tcnative.

### Step 2: Install netty-tcnative

3.1) Add the netty-tcnative binary to the local maven repository

```bash
mvn install:install-file -DgroupId=io.netty -DartifactId=netty-tcnative-boringssl-static -Dversion=2.0.20.Final -Dclassifier=linux_s390_64 -Dpackaging=jar -Dfile=$SOURCE_ROOT/netty-tcnative/boringssl-static/target/netty-tcnative-boringssl-static-2.0.27.Final-SNAPSHOT-linux-s390_64.jar
```

---


## Part 4: Build fabric-sdk-java

### Step 1: Install dependencies

```bash
export SOURCE_ROOT=/<source_root>/
```

- Ubuntu (16.04)

```bash
sudo apt-get update 
sudo apt-get install tar wget autoconf automake g++ git gzip libtool make bzip2 curl unzip zlib1g-dev openjdk-8-jdk-headless maven ninja-build cmake libssl-dev perl golang libapr1-dev

```

### Step 2: Download and install fabric-sdk-java

2.1) Clone the GitHub repository

```bash
cd $SOURCE_ROOT
git clone https://github.com/Henrywelborn/fabric-sdk-java.git
cd $SOURCE_ROOT/fabric-sdk-java
git checkout HenryWelborn-patch-1
```

2.2) Build for s390x. **WORK IN PROGRESS**

```bash
cd $SOURCE_ROOT/fabric-sdk-java
mvn package
```

### Step 3: Install fabric-sdk-java

3.1) TBD