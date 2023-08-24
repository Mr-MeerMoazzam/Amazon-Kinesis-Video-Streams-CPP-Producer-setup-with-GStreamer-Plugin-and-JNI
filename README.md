# Amazon Kinesis Video Streams Producer SDK for C++

The Amazon Kinesis Video Streams Producer SDK for C++ allows you to build applications that securely connect to video streams, reliably publish video and other media data to Kinesis Video Streams, and perform various tasks related to streaming and video processing.

## Features

- C++ SDK
- GStreamer Plugin (kvssink)
- JNI (Java Native Interface)

## Introduction

The Amazon Kinesis Video Streams Producer SDK for C++ simplifies the process of building on-device applications that handle video streaming. It handles stream creation, token rotation, acknowledgments from Kinesis Video Streams, and other necessary tasks.

## Getting Started

### Download

To clone the repository, run the following command:

```bash
git clone https://github.com/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp.git
```

Note: Make sure you have `java` setup, `pkg-config`, `CMake`, `m4` and a build environment in ubunto before moving forward. As You are going to build the GStreamer plugin you will also need GStreamer and GStreamer (Development Libraries) in my system.

Dependencies Installation in Ubunto:

```
sudo apt-get install libssl-dev libcurl4-openssl-dev liblog4cplus-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-bad gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-tools
```


### Configure

Prepare a build directory in the newly checked out repository:
```
mkdir -p amazon-kinesis-video-streams-producer-sdk-cpp/build

cd amazon-kinesis-video-streams-producer-sdk-cpp/build
```

GStreamer and JNI is NOT built by default, if you wish to build both you MUST execute 

```cmake .. -DBUILD_GSTREAMER_PLUGIN=ON -DBUILD_JNI=TRUE```

### Compiling

After running cmake, in the same build directory run make:

```
make
```

## Run
### GStreamer Plugin (kvssink)
#### Loading Element
The GStreamer plugin is located in your build directory. To load this plugin set the following environment variables. This should be run from the root of the repo, NOT the build directory.
1. Firstly run these commands to enter super user mode.(Because if you load on root it will give you errors)
```
sudo su
nano ~/.bashrc
```
2. Got to the end of the file and paste these lines with actual paths of your system in place of 'pwd'
```
export GST_PLUGIN_PATH=`pwd`/build
export LD_LIBRARY_PATH=`pwd`/open-source/local/lib
export aws_access_key_id= "your aws IAM user access key id"
export aws_secret_access_key= "your aws IAM user secret access key"
```
3. Now if you execute `gst-inspect-1.0 kvssink` in your super user privileges you should get information on the plugin like
```
Factory Details:
  Rank                     primary + 10 (266)
  Long-name                KVS Sink
  Klass                    Sink/Video/Network
  Description              GStreamer AWS KVS plugin
  Author                   AWS KVS <kinesis-video-support@amazon.com>

Plugin Details:
  Name                     kvssink
  Description              GStreamer AWS KVS plugin
  Filename                 /Users/seaduboi/workspaces/amazon-kinesis-video-streams-producer-sdk-cpp/build/libgstkvssink.so
  Version                  1.0
  License                  Proprietary
  Source module            kvssinkpackage
  Binary package           GStreamer
  Origin URL               http://gstreamer.net
```

If the build failed, or GST_PLUGIN_PATH is not properly set you will get output like

```
No such element or plugin 'kvssink'
```
### kvsink Element Usage
The kvssink element has the following required parameters:

- `stream-name` -- The name of the destination Kinesis video stream.
- `storage-size` -- The storage size of the device in megabytes. For information about configuring device storage, see StorageInfo.
- `access-key` -- The AWS access key that is used to access Kinesis Video Streams. You must provide either this parameter or credential-path.
- `secret-key` -- The AWS secret key that is used to access Kinesis Video Streams. You must provide either this parameter or credential-path.
- `credential-path` -- A path to a file containing your credentials for accessing Kinesis Video Streams. For example credential files, see Sample Static Credential and Sample Rotating Credential. For more information on rotating credentials, see Managing Access Keys for IAM Users. You must provide either this parameter or access-key and secret-key.

For examples of common use cases, you can look at Example:[Kinesis Video Straem Examples](https://docs.aws.amazon.com/kinesisvideostreams/latest/dg/examples-gstreamer-plugin.html)

