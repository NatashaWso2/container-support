# Ballerina Base Docker Image

This Dockerfile can be used to create a Ballerina base Docker distribution that can be used to run Ballerina archives and package Ballerina archives as distributable Docker images.

## Usage
The Docker distribution for Ballerina is available on Docker Hub as `ballerinalang/ballerina`. To run a Ballerina package using the Ballerina Docker image, simply mount the folder containing the package(s) to `/ballerina/files` folder inside the container. 

```bash
# Create a directory and copy the packages needed to be run.
mkdir -p ~/ballerina/packages/
cp mypackage.bmz ~/ballerina/packages/
chmod +r ~/ballerina/packages/mypackage.bmz

# Run container with the volume mount
docker run -v ~/ballerina/packages:/ballerina/files -it ballerinalang/ballerina:0.8.3
```

If you are running a Ballerina Service, set the `SVC_MODE` environment variable to `true`. 

```bash
cp mysvcpackage.bsz ~/ballerina/packages/
chmod +r ~/ballerina/packages/mysvcpackage.bsz

docker run -v /tmp/testb:/ballerina/files -e "SVC_MODE=true" -p 9090:9090 -it ballerinalang/ballerina:0.8.3
```

## Building the image
```bash
# bash build.sh -d <ballerina-dist> to build ballerina:latest
cp ~/Downloads/ballerina-0.8.3.zip .
bash build.sh -d ballerina-0.8.3.zip

# Or build without copying the distribution.
bash build.sh -d ~/Downloads/ballerina-0.8.3.zip 

# Show usage
bash build.sh -h
```

## As a Parent Image
This image contains build triggers to make sure crucial settings are applied in the child images. Because of this, the following values must be set as build arguments when building child images.

```
SVC_MODE=[true|false]
FILE_MODE=[true|false]
BUILD_DATE=[RFC3339 formatted date]
```

Out of these, `BUILD_DATE` is mandatory, as this affects the meta data of the child images. The value should be `RFC 3339` formatted value of the current date and time.

```
BUILD_DATE=2017-02-27T09:22:57Z
```
