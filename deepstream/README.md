
# DeepStream SDK Python Bindings

## Setup pre-requisites:
- Ubuntu 22.04
- NVIDIA DeepStream SDK 6.4
- Python 3.10
- Gst-python

--------------------------------------------------------------------------------
Package Contents
--------------------------------------------------------------------------------
1. DeepStream Python bindings located in bindings dir
   with installation instructions in bindings/README.md

2. DeepStream test apps in Python

--------------------------------------------------------------------------------
Installing Pre-requisites:
--------------------------------------------------------------------------------
--------------------------------------------------------------------------------
Running the samples inside DeepStream SDK docker
--------------------------------------------------------------------------------
The general steps are:
1. Pull the DeepStream SDK docker of choice following the latest DeepStream
   Release Notes at https://developer.nvidia.com/deepstream-sdk for more info.
   Note that the deepstream-ssd-parser app requires the Triton docker on x86_64.
2. Run the docker with Python Bindings mapped using the following option:
   -v <path to this python bindings directory>:/opt/nvidia/deepstream/deepstream/sources/python
3. Inside the container, install packages required by all samples:
   $ sudo apt update
   $ sudo apt install python3-gi python3-dev python3-gst-1.0 -y
4. Optionally install additional dependencies required by specific samples.
   See README in app's directory for such requirements.
   $ sudo apt install python3-opencv
   $ sudo apt install python3-numpy
   $ sudo apt install libgstrtspserver-1.0-0 gstreamer1.0-rtsp
   $ sudo apt install libgirepository1.0-dev
   $ sudo apt install gobject-introspection gir1.2-gst-rtsp-server-1.0
5. Run sample apps following directions in each app's README.

--------------------
DeepStream SDK 6.4
--------------------
Download and install from https://developer.nvidia.com/deepstream-download
Or use the docker file.

Python 3.10
----------
Should be already installed with Ubuntu 22.04

Gst-python
----------
Should be already installed on Jetson
If missing, install with the following steps:
$ sudo apt update
$ sudo apt install python3-gi python3-dev python3-gst-1.0 -y


--------------------------------------------------------------------------------
Running the samples
--------------------------------------------------------------------------------
The apps are configured to work from inside the DeepStream SDK 6.4 installation.

Clone the deepstream_python_apps repo under <DeepStream ROOT>/sources:
$ git clone https://github.com/NVIDIA-AI-IOT/deepstream_python_apps
This will create the following directory:
<DeepStream ROOT>/sources/deepstream_python_apps

Follow README in each app's directory to run the app.

Example: running test1 app:
$ cd deepstream-test1
$ python3 deepstream_test_1.py <input .h264 file>

--------------------------------------------------------------------------------
Notes:
--------------------------------------------------------------------------------
As with DeepStream SDK, if the application runs into errors, cannot create gst elements, 
try again after removing gstreamer cache
   rm ${HOME}/.cache/gstreamer-1.0/* 