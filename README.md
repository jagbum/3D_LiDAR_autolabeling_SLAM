# 3D_LiDAR_autolabeling-SLAM



<a href="https://youtu.be/xc8WTAzaojE?si=GdG4wGwFMSqKU9z7" target="_blank"><img width="480" height="360" border="10" alt="3D_LiDAR_autolabeling_SLAM" src="https://github.com/HeyLip/3D_LiDAR_autolabeling_SLAM/assets/102511107/39f66267-45f9-4dff-bef9-d29c6f4fe9b8">

# 1. Building 3D_LiDAR_autolabeling-SLAM

Clone the repository:
```
git clone https://github.com/HeyLip/3D_LiDAR_autolabeling_SLAM.git
```

## Building script
For your convenience, we provide a building script `build_cuda113.sh` which show step-by-step how 3D_LiDAR_autolabeling-SLAM is built and which dependencies are required. Those scripts will install everything for you including CUDA (version is specified in the script name)

You can simply run:

```
./build_cuda113.sh --install-cuda --build-dependencies --create-conda-env
```

and it will set up all the dependencies and build 3D_LiDAR_autolabeling-SLAM for you. If you want to have a more flexible installation (use your own CUDA and Pytorch, build 3D_LiDAR_autolabeling-SLAM with your own version of OpenCV, Eigen3, etc), Those scripts can also provide important guidance for you.


## CMake options:
When building 3D_LiDAR_autolabeling-SLAM the following CMake options are mandatory: `PYTHON_LIBRARIES`, `PYTHON_INCLUDE_DIRS`, `PYTHON_EXECUTABLE`. Those must correspond to the same Python environment where your dependencies (PyTorch, mmdetection, mmdetection3d) are installed. Make sure these are correctly specified!

Once you have set up the dependencies, you can build 3D_LiDAR_autolabeling-SLAM: 

```
# (assume you are under 3D_LiDAR_autolabeling-SLAM project directory)
mkdir build
cd build
cmake -DPYTHON_LIBRARIES={YOUR_PYTHON_LIBRARY_PATH} \
      -DPYTHON_INCLUDE_DIRS={YOUR_PYTHON_INCLUDE_PATH} \
      -DPYTHON_EXECUTABLE={YOUR_PYTHON_EXECUTABLE_PATH} \
      ..
make -j8
```

After successfully building 3D_LiDAR_autolabeling-SLAM, you will have **lib3D_LiDAR_autolabeling-SLAM.so**  at *lib* folder and the executables **3D_LiDAR_autolabeling_slam** and under project root directory.

# 2. Running 3D_LiDAR_autolabeling-SLAM

## Dataset
You can download the example sequences and pre-trained network model weights (PointPillars) from [here](https://liveuclac-my.sharepoint.com/:f:/g/personal/ucabjw4_ucl_ac_uk/Eh3nHv6D-LZHkuny4iNOexQBGdDVxloM_nwbEZdxeRfStw?e=sYO1Ot). It contains example sequences of [KITTI](http://www.cvlibs.net/datasets/kitti/eval_odometry.php)

## Run 3D_LiDAR_autolabeling_slam

After obtaining the 2 binary executables, you will need to suppy 4 parameters to run the program: 1. path to vocabulary 2. path to .yaml config file 3. path to sequence data directory 4. path to save map. Before running 3D_LiDAR_autolabeling-SLAM, make sure you run `conda activate 3D_LiDAR_autolabeling-slam` to activate the correct Python environmrnt. Here are some example usages:

For KITTI sequence for example, you can run:

```
./3D_LiDAR_autolabeling_slam Vocabulary/ORBvoc.bin configs/KITTI04-12.yaml data/kitti/07 map/kitti/07
```

### Run 3D_LiDAR_autolabeling-SLAM with offline detector
If you can successfully build 3D_LiDAR_autolabeling-SLAM but get errors from Python side when running the program, then you can try supplying pre-stored labels and run 3D_LiDAR_autolabeling-SLAM with offline detector. We have provided 3D labels for KITTI sequence in the [data](https://liveuclac-my.sharepoint.com/:f:/g/personal/ucabjw4_ucl_ac_uk/Eh3nHv6D-LZHkuny4iNOexQBGdDVxloM_nwbEZdxeRfStw?e=sYO1Ot). To run 3D_LiDAR_autolabeling-SLAM with offline mode, you will need to change the field `detect_online` in the .json config file to `false` and specify the corresponding label path.

### Label format
If you want to create your own labels with your own detectors, you can follow the same format as the labels we provided in the KITTI-07 sequence.
* 3D labels contains 3D detection boxes under [KITTI](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d) convention. Each `.lbl` file consits of a numpy array of size Nx7, where N is the number of objects detected. Each row of the array is a 3D detection box: [x, y, z, w, l, h, ry]. More information about the KITTI coordinate system can be found from [mmdetection3d](https://github.com/open-mmlab/mmdetection3d) or [KITTI website](http://www.cvlibs.net/publications/Geiger2013IJRR.pdf).
* 2D labels contains MaskRCNN detection boxes and segmentation masks. Each `.lbl` file consists of of a dictionary with two keys: `pred_boxes` and `pred_masks`. Boxes and masks are stored as numpy array of size Nx4 and NxHxW.

# 3. License
3D_LiDAR_autolabeling-SLAM includes the part of third-party open-source software DSP-SLAM, which itself includes third-party open-source software.
DSP-SLAM includes the third-party open-source software ORB-SLAM2, which itself includes third-party open-source software. Each of these components have their own license.
3D_LiDAR_autolabeling-SLAM is released under [GPLv3 license](LICENSE) in line with ORB-SLAM2. For a list of all code/library dependencies (and associated licenses), please see [Dependencies.md](Dependencies.md).

# 4. Acknowledgements
Research presented here has been supported by the UCL Centre for Doctoral Training in Foundational AI under UKRI grant number EP/S021566/1. We thank [Wonbong Jang](https://sites.google.com/view/wbjang/home) and Adam Sherwood for fruitful discussions. We are also grateful to [Binbin Xu](https://www.doc.ic.ac.uk/~bx516/) and [Xin Kong](https://kxhit.github.io/) for their patient code testing!

Thank you DSP_SLAM, JingwenWang95, https://github.com/JingwenWang95/DSP-SLAM.git
