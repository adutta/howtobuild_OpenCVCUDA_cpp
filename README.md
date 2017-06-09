# How to Build OpenCV with CUDA
How to build OpenCV with CUDA for C++ on Windows

# Intro
Does your Windows PC have an GPU made by NVIDIA?

Good news: you can use those CUDA cores with OpenCV! Even the weakest GPUs can boost your computation time.
Bad news: You can't download this. You will need to build OpenCV to work with your GPU. 
Worse news: There's so many options that you might be overwhelmed by what to and not to do.

This guide is intended to help you quickly figure out what you need to do to build OpenCV so it can leverage your CUDA cores. This was created with a bit of trial and error.

# Prerequisites:
## PC Requirements
* 64-bit Windows OS
* An NVIDIA GPU with CUDA cores
* ~5GB of space free on a single drive

## Software
Before you start, there's a few things that you will need to download and install for your machine:
  
  * CMake: https://cmake.org/
  * Visual Studio for C++ (Community/Express will work): https://www.visualstudio.com/ 
  * CUDA Toolkit: https://developer.nvidia.com/cuda-toolkit
  * CUDA Tools SDK (Maybe? 4.0 is the latest I can find...): https://developer.nvidia.com/cuda-toolkit-40

### Visual Studio Notes
* The version of Visual Studio you need will be dependent on your compatibility requirements.
* Make sure you download the C++ support and it can build 64-bit. 
* Make sure VS it's in your path as well.

## Download OpenCV
Get the latest relase. This contains your source code.

Link: http://opencv.org/releases.html

# Generate Code with CMake
CMake is a GUI tool to configure OpenCV to your liking before it's built.

1. Point CMake to where the source code is. Look for the directory with the CMakeLists.txt file in it.
2. Hit configure

![alt text](https://github.com/adutta/howtobuild_OpenCVCUDA_cpp/blob/master/cmake1.PNG "Should see something like this after hitting configure.")

3.  Check the following:
 
  * WITH_CUDA
  * WITH_CUBLAS
  * CUDA_FAST_MATH 

   Check **only** if you do not care about absolute accuracy. The tradeoff here is accuracy for speed.

  * BUILD_opencv_world 

   Select **only** if you don't plan to deploy this. This will build everything into one lib/dll. This is not lean at all.

  * INSTALL_TESTS 

   Lets you verify everything works.

4. Uncheck the BUILD_opencv_python2 flag if you don't plan on using python

  * Otherwise you will need Python with debug built on your machine

![alt text](https://github.com/adutta/howtobuild_OpenCVCUDA_cpp/blob/master/cmake2.PNG "Check all that apply.")

5. Hit configure again/until the red text highlights go away
6. **Finally**: Hit Generate button to build C++ code

# Build with Visual Studio
The code has now been configured as you need it. Now we actually get to build OpenCV.

1. **MAKE SURE BUILDING 64 BIT**

![alt text](https://github.com/adutta/howtobuild_OpenCVCUDA_cpp/blob/master/build_64.PNG "Building 64-bit is critical.")

2. Build debug **and** release
3. Build ALL_BUILD
4. Build INSTALL

| | |
---|---
![alt text](https://github.com/adutta/howtobuild_OpenCVCUDA_cpp/blob/master/vs_build1.png "Where to find these.")|![alt text](https://github.com/adutta/howtobuild_OpenCVCUDA_cpp/blob/master/vs_build2.png "Right click to build.")

  * Expect **2 hours** to build each ALL_BUILD.
  * Change your power settings temporarily so your computer doesn't fall asleep during this.

6. Look for [build directory]/install/ for dlls, libs, test programs for debug and release
7. Review OpenCVConfig.cmake file to double check how things were configured

# Test
Run a subset (or all if you really want to ) of the test executables to see if things built correctly
* Have the lena.png and lena.jpg in the exe directory. Some tests will need these.
* cvcore, cudarithm should be tested

# Move OpenCV Where You Want
Move your new opencv to the place you want (C drive maybe?)
* Add opencv\bin to your path (has all the dlls)

# Setting Up Visual Studio Project
This part will be very similar to what OpenCV's documentation has. Link to 2.4 documentation in the reference section below. I use the local method here, but you can use the global method if you wish.

1. Build for 64 bit only. This is a requirement to use the GPU.
2. Include debug and release libs that you require
3. Edit project for C/C++: include additional libraries
4. Edit project for Linker: include opencv lib
5. Add additional libraries under Linker
6. Repeat for release building

If Visual Studio complains about pdb symbols not being found, you may need to grab symbols from Microsoft for debugging:
* Tools->Options->Debugging->Symbols and select checkbox "Microsoft Symbol Servers"

## Notes:
* Firewalls/proxys may make things tricky for you! Manually downloading dlls and other items may be required.
* Remember that to use the GPU, you must first download matricies to the GPU. Once you're finished, upload back to the CPU.

# References/Thanks:
* https://initialneil.wordpress.com/2014/09/25/opencv-2-4-9-cuda-6-5-visual-studio-2013/
* http://docs.opencv.org/2.4/doc/tutorials/introduction/windows_visual_studio_Opencv/windows_visual_studio_Opencv.html
