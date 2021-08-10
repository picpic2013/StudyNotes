# Ubuntu 下安装 OpenCV

## 克隆 opencv + opencv_contrib

~~~ shell
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
~~~

## 新建 build 文件夹

~~~ shell
mkdir opencv/build
cd opencv/build
~~~

## 安装编译用的依赖

~~~ shell
sudo apt update

# Generic tools
sudo apt install build-essential cmake pkg-config unzip yasm git checkinstall

# Image I/O libs
sudo apt install libjpeg-dev libpng-dev libtiff-dev

# Video/Audio Libs - FFMPEG, GSTREAMER, x264 and so on
sudo apt install libavcodec-dev libavformat-dev libswscale-dev libavresample-dev
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
sudo apt install libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev 
sudo apt install libfaac-dev libmp3lame-dev libvorbis-dev

# OpenCore - Adaptive Multi Rate Narrow Band (AMRNB) and Wide Band (AMRWB) speech codec
sudo apt install libopencore-amrnb-dev libopencore-amrwb-dev

# Cameras programming interface libs
sudo apt-get install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils
sudo ln -s -f /usr/include/libv4l1-videodev.h /usr/include/linux/videodev.h

# GTK lib for the graphical user functionalites coming from OpenCV highghui module
sudo apt-get install libgtk-3-dev

# Parallelism library C++ for CPU
sudo apt-get install libtbb-dev

# Optimization libraries for OpenCV
sudo apt-get install libatlas-base-dev gfortran

# Optional libraries
sudo apt-get install libprotobuf-dev protobuf-compiler
sudo apt-get install libgoogle-glog-dev libgflags-dev
sudo apt-get install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen
~~~

## 安装 Anaconda & Python环境

## 执行安装流程

### cmake

~~~ shell
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D WITH_TBB=ON \
-D ENABLE_FAST_MATH=1 \
-D CUDA_FAST_MATH=1 \
-D WITH_CUBLAS=1 \
-D WITH_CUDA=ON \
-D BUILD_opencv_cudacodec=OFF \
-D WITH_CUDNN=ON \
-D OPENCV_DNN_CUDA=ON \
-D CUDA_ARCH_BIN=7.5 \
-D WITH_V4L=ON \
-D WITH_QT=OFF \
-D WITH_OPENGL=ON \
-D WITH_GSTREAMER=ON \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D OPENCV_PC_FILE_NAME=opencv.pc \
-D OPENCV_ENABLE_NONFREE=ON \
-D OPENCV_PYTHON3_INSTALL_PATH=/home/pic/anaconda3/envs/opencv/lib/python3.9/site-packages \
-D PYTHON_EXECUTABLE=/home/pic/anaconda3/envs/opencv/bin/python \
-D OPENCV_EXTRA_MODULES_PATH=/home/pic/opencv/opencv_contrib/modules \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D INSTALL_C_EXAMPLES=ON \
-D BUILD_EXAMPLES=OFF \
-D CUDNN_INCLUDE_DIR=/usr/local/cuda/include \
-D CUDNN_LIBRARY=/usr/local/cuda/lib64/libcudnn.so.8.2.2 \
..
~~~

注意替换以下变量

~~~ shell
# Python 的 site-package 路径
OPENCV_PYTHON3_INSTALL_PATH

# Python 执行路径
PYTHON_EXECUTABLE

# opencv_contrib 引用文件路径
OPENCV_EXTRA_MODULES_PATH

# CUDADNN 安装路径
CUDNN_INCLUDE_DIR

# libcudnn.so.8.2.2 的位置
CUDNN_LIBRARY
~~~

检查输出，`CUDNN`等是否为`YES`

### make

~~~ shell
nproc
make -j8
sudo make install
sudo /bin/bash -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/opencv.conf'
sudo ldconfig
~~~

## 安装 VSCode插件



### C++编译环境的配置

给vs code安装C/C++插件

Debug -> Open Configurations -> 打开备选框 -> C++(GDB/LLDB) -> g++ build and debug active file

上述操作打开launch.json文件，修改其中的设置项。修改后如下：

~~~ json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++ build file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [{
                "description": "Enable pretty-printing for gdb",
                "text": "-enable-pretty-printing",
                "ignoreFailures": true
            }],
            "preLaunchTask": "g++ build active file",
            "miDebuggerPath": "/usr/bin/gdb"
        }, {
            "name": "g++ build group",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [{
                "description": "Enable pretty-printing for gdb",
                "text": "-enable-pretty-printing",
                "ignoreFailures": true
            }],
            "preLaunchTask": "g++ build active group",
            "miDebuggerPath": "/usr/bin/gdb"
        }, {
            "name": "opencv build group",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "setupCommands": [{
                "description": "Enable pretty-printing for gdb",
                "text": "-enable-pretty-printing",
                "ignoreFailures": true
            }],
            "preLaunchTask": "opencv build active group",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
~~~

其中需要重点注意的几个地方在于：
- `"externalConsole": true`关键字修改为`true`，指的是弹出命令窗口。
- `"preLaunchTask": "g++ build active file"` 后面的配置需要用到这个关键字。

随便写点C++代码，点击F5运行

选择Configure Task，在备选框中选择C/C++:cpp build active file选项，则新建一个task.json文件。

修改文件配置如下：

~~~ json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "g++ build active file",
            "command": "g++",
            "args": [
                "-g",
                "-std=c++11",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "options": {
                "cwd": "/usr/bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        }, {
            "type": "shell",
            "label": "g++ build active group",
            "command": "g++",
            "args": [
                "-g",
                "-std=c++11",
                "${fileDirname}/*.cpp",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "options": {
                "cwd": "/usr/bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        }, {
            "type": "shell",
            "label": "opencv build active group",
            "command": "g++",
            "args": [
                "-g",
                "-std=c++11",
                "${fileDirname}/*.cpp",
                "${fileDirname}/*/*.cpp",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}",
                "-I", "/usr/local/include/opencv4",
                "-L", "/usr/local/lib",
                "-l", "opencv_aruco",
                "-l", "opencv_bgsegm",
                "-l", "opencv_bioinspired",
                "-l", "opencv_calib3d",
                "-l", "opencv_ccalib",
                "-l", "opencv_core",
                "-l", "opencv_datasets",
                "-l", "opencv_dnn_objdetect",
                "-l", "opencv_dnn",
                "-l", "opencv_dpm",
                "-l", "opencv_face",
                "-l", "opencv_features2d",
                "-l", "opencv_flann",
                "-l", "opencv_freetype",
                "-l", "opencv_fuzzy",
                "-l", "opencv_hfs",
                "-l", "opencv_highgui",
                "-l", "opencv_imgcodecs",
                "-l", "opencv_img_hash",
                "-l", "opencv_imgproc",
                "-l", "opencv_line_descriptor",
                "-l", "opencv_ml",
                "-l", "opencv_objdetect",
                "-l", "opencv_optflow",
                "-l", "opencv_phase_unwrapping",
                "-l", "opencv_photo",
                "-l", "opencv_plot",
                "-l", "opencv_reg",
                "-l", "opencv_rgbd",
                "-l", "opencv_saliency",
                "-l", "opencv_shape",
                "-l", "opencv_stereo",
                "-l", "opencv_stitching",
                "-l", "opencv_structured_light",
                "-l", "opencv_superres",
                "-l", "opencv_surface_matching",
                "-l", "opencv_text",
                "-l", "opencv_tracking",
                "-l", "opencv_videoio",
                "-l", "opencv_video",
                "-l", "opencv_videostab",
                "-l", "opencv_xfeatures2d",
                "-l", "opencv_ximgproc",
                "-l", "opencv_xobjdetect",
                "-l", "opencv_xphoto"
            ],
            "options": {
                "cwd": "/usr/bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": "build"
        }
    ]
}
~~~

其中，需要注意的几点是：
- `"label": "g++ build active file"` 关键字必须与前述`launch.json`文件的`"preLaunchTask": "g++ build active file"` 关键字一致。
- `"command": "g++"` 关键字要修改为g++，否则可能会报不能debug的提示。
- `"args": [ "-g", "-std=c++11", "${file}", "-o", "${fileDirname}/${fileBasenameNoExtension}" ]` 关键字就是g++的编译指令。

设置给编译器

Ctrl + Shift + P 打开搜索框，键入c++，会出现备选项目，选择图示Edit configurations (JSON)

进入c_cpp_properties.json文件内进行配置修改。修改如下：
~~~ json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "/usr/local/include/opencv4"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu17",
            "cppStandard": "gnu++14",
            "intelliSenseMode": "linux-gcc-x64"
        }
    ],
    "version": 4
}
~~~

## 参考资料

- [How to install OpenCV 4.5.2 with CUDA 11.2 and CUDNN 8.2 in Ubuntu 20.04](https://gist.github.com/raulqf/f42c718a658cddc16f9df07ecc627be7)
- [Ubuntu环境下VScode配置OpenCV的C++开发环境](https://blog.csdn.net/sunzhao1000/article/details/103185875/)