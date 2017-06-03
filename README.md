# How to Build OpenCV with CUDA
How to build OpenCV with CUDA for C++ on Windows

# Intro
Does your Windows PC have an GPU made by NVIDIA?

Good news: you can use those CUDA cores with OpenCV! Even the weakest GPUs can boost your computation time.
Bad news: You can't download this. You will need to build OpenCV to work with your GPU. 
Worse news: There's so many options that you might be overwhelmed by what to and not to do.

This guide is intended to help you quickly figure out what you need to do to build OpenCV so it can leverage your CUDA cores.

# Prerequisites:
## PC Requirements
* 64-bit Windows
* An NVIDIA GPU with CUDA cores
* ~5GB of space free on a single drive

## Software
Before you start, there's a few things that you will need to download and install for your machine:
* CMake: https://cmake.org/
* Visual Studio for C++ (Community/Express will work): https://www.visualstudio.com/ ==> Make sure it's in your path
* CUDA Toolkit (latest you can get): https://developer.nvidia.com/cuda-toolkit
* CUDA Tools (maybe?...4.0 is the latest I can find): https://developer.nvidia.com/cuda-toolkit-40

The version of Visual Studio you need will be dependent on your compatibility requirements. Make sure you download the C++ support and it can build 64-bit.

# GENERATE CODE WITH CMAKE
1. Hit configure
2.  Check the following:
* WITH_CUDA
* WITH_CUBLAS
* CUDA_FAST_MATH ===> ONLY IF ACCURACY IS NOT CRITICAL (tradeoff: speed and accuracy)
* BUILD_opencv_world ====> ONLY IF YOU DON"T PLAN TO DEPLOY...THIS IS ALL OF OPENCV IN ONE LIB/DLL...NOT LEAN
* INSTALL_TESTS ==> LETS YOU VERIFY EVERYTHING WORKS 
3. Uncheck the BUILD_opencv_python2 flag if you don't plan on using python (otherwise you will need python with debug built on your machine)
4. Hit configure again/until the red text highlights go away
5. FINALLY: Hit Generate button to build C++ code

# BUILD WITH VISUAL STUDIO
1. MAKE SURE BUILDING 64 BIT
2. Build debug AND release
3. Build ALL_BUILD
4. Build INSTALL
5. Expect 2 hours to build each ALL_BUILD
6. look for [build directory]/install/ for dlls, libs, test programs for debug and release
7. Review OpenCVConfig.cmake file to double check how things were configured

# TEST
* Run a subset (or all if you really want to ) of the test executables to see if things built correctly
* Have the lena.png and lena.jpg in the exe directory. Some tests will need these.
* cvcore, cudarithm should be tested

# MOVE WHERE YOU WANT
1. Move your new opencv to the place you want (C drive maybe?)
2. Add opencv\bin to your path (has all the dlls)

# SETTING UP VISUAL STUDIO CODE
This part will be very similar to what OpenCV's documentation has. Link to 2.4 documentation in the reference section below.

1. Build for 64 bit only
2. Include debug and release libs that you require
3. Edit project for C/C++: include additional libraries
4. Edit project for Linker: include opencv lib
5. Add additional libraries under Linker
6. Repeat for release building
7. May need to grab symbols from Microsoft for debugging (if it complains about pdb symbols not found)
* Tools->Options->Debugging->Symbols and select checkbox "Microsoft Symbol Servers"

## Notes:
Firewalls/proxys may make things tricky for you! Manually downloading dlls and other items may be required

# References/Thanks:
* https://initialneil.wordpress.com/2014/09/25/opencv-2-4-9-cuda-6-5-visual-studio-2013/
* http://docs.opencv.org/2.4/doc/tutorials/introduction/windows_visual_studio_Opencv/windows_visual_studio_Opencv.html
