[简体中文](README.md) | English

## Project Introduction

![License](https://img.shields.io/badge/License-Apache%202-red.svg)
![python version](https://img.shields.io/badge/Python-3.11+-blue.svg)
![support os](https://img.shields.io/badge/OS-Windows/macOS/Linux-green.svg)

Video-subtitle-remover (VSR) is an AI-based software that removes hardcoded subtitles from videos. It mainly implements the following functionalities:

- **Lossless resolution**: Removes hardcoded subtitles from videos and generates files without subtitles.
- Fills in the removed subtitle text area using a powerful AI algorithm model (non-adjacent pixel filling and mosaic removal).
- Supports custom subtitle positions by only removing subtitles in the defined location (input position).
- Supports automatic removal of all text throughout the entire video (without inputting a position).
- Supports multi-selection of images for batch removal of watermark text.

<p style="text-align:center;"><img src="https://github.com/YaoFANGUK/video-subtitle-remover/raw/main/design/demo.png" alt="demo.png"/></p>

> Download the .zip package directly, extract, and run it. If it cannot run, follow the tutorial below to try installing the conda environment and running the source code.


**forked from [YaoFANGUK/video-subtitle-remover](https://github.com/YaoFANGUK/video-subtitle-remover)** 

On the basis of the original project, it supports CPU (M1) , ~~GPU (AMD)~~ running, and some minor changes.

 
## Demonstration

- GUI：

<p style="text-align:center;"><img src="https://github.com/YaoFANGUK/video-subtitle-remover/raw/main/design/demo2.gif" alt="demo2.gif"/></p>

- <a href="https://b23.tv/guEbl9C">点击查看演示视频👇</a>

<p style="text-align:center;"><a href="https://b23.tv/guEbl9C"><img src="https://github.com/YaoFANGUK/video-subtitle-remover/raw/main/design/demo.gif" alt="demo.gif"/></a></p>

## Source Code Usage Instructions

this branch runs on CPU (M1 Apple M1 16G) , ~~GPU (AMD)~~  

#### 1. Download and install conda(Miniconda or Anaconda)

- Windows: <a href="https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe">Miniconda3-latest-Windows-x86_64.exe</a>
- Linux: <a href="https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh">Miniconda3-latest-Linux-x86_64.sh</a>
- Linux: <a href="https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh">Miniconda3-latest-MacOSX-x86_64.sh</a>


#### 2. Create and activate a virtual environment

1. Switch to the source code directory：
```shell
cd <source_code_directory>
```
> For example, if your source code is in a user directory and the folder name of the source code is video-subtitle-remover, enter cd ~/video-subtitle-remover-main'''
2. Create and activate the conda environment:
```shell
conda create -n video-subtitle-remover python=3.11.7
```

```shell
conda activate video-subtitle-remover
```

#### 3. Install dependencies

 

- 安装CPU版本Paddlepaddle:

  - * macOS:

      ```shell 
      conda install paddlepaddle==2.6.0 --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/Paddle/
    
      ```
    * or Choose the installation based on the operating system, installation method, and computing platform:
      [quick install](https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/install/pip/macos-pip.html)

- install the cpu version of pytorch:
      
  ```shell
  conda install pytorch::pytorch torchvision torchaudio -c pytorch
  ```

  - or [selectt OS, Package, And so.. insta](https://pytorch.org/get-started/locally/)
 

- Install additional dependencies:

  ```shell
  pip install -r requirements.txt
  ```


#### 4. Run the program

- Run the GUI

```shell
python gui.py
```

- Run the command line version (CLI)

```shell
python ./backend/main.py
```

## Common Issues

1. How to deal with slow removal speed


You can greatly increase the removal speed by modifying the parameters in backend/config.py:

```python
MODE = InpaintMode.STTN  # 设置为STTN算法
STTN_SKIP_DETECTION = True # 跳过字幕检测，跳过后可能会导致要去除的字幕遗漏或者误伤不需要去除字幕的视频帧
```

2. What to do if the video removal results are not satisfactory

Modify the values in backend/config.py and try different removal algorithms. Here is an introduction to the algorithms:

> - InpaintMode.STTN algorithm: Good for live-action videos and fast in speed, capable of skipping subtitle detection
> - InpaintMode.LAMA algorithm: Best for images and effective for animated videos, moderate speed, unable to skip subtitle detection
> - InpaintMode.PROPAINTER algorithm: Consumes a significant amount of VRAM, slower in speed, works better for videos with very intense movement

- Using the STTN algorithm


```python
MODE = InpaintMode.STTN  # Set to STTN algorithm
# Number of neighboring frames, increasing this will increase memory usage and improve the result
STTN_NEIGHBOR_STRIDE = 10
# Length of reference frames, increasing this will increase memory usage and improve the result
STTN_REFERENCE_LENGTH = 10
# Set the maximum number of frames processed simultaneously by the STTN algorithm, a larger value leads to slower processing but better results
# Ensure that STTN_MAX_LOAD_NUM is greater than STTN_NEIGHBOR_STRIDE and STTN_REFERENCE_LENGTH
STTN_MAX_LOAD_NUM = 30
```
- Using the LAMA algorithm

```python
MODE = InpaintMode.LAMA  # Set to LAMA algorithm
LAMA_SUPER_FAST = False  # Ensure quality
```