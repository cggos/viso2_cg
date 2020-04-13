# viso2_cg

Modified version of LIBVISO2 downloaded from [this](http://www.cvlibs.net/downloads/libviso2.zip).

[LIBVISO2: C++ Library for Visual Odometry 2](http://www.cvlibs.net/software/libviso/) is a very fast cross-platfrom (Linux, Windows) C++ library with MATLAB wrappers for computing the 6 DOF motion of a moving mono/stereo camera. For its ROS wiki, please visit [here](http://www.ros.org/wiki/viso2); 

* The stereo version is based on minimizing the reprojection error of sparse feature matches and is rather general (no motion model or setup restrictions except that the input images must be rectified and calibration parameters are known).   
  
* The monocular version is still very experimental and uses the 8-point algorithm for fundamental matrix estimation. It further assumes that the camera is moving at a known and fixed height over ground (for estimating the scale). Due to the 8 correspondences needed for the 8-point algorithm, many more RANSAC samples need to be drawn, which makes the monocular algorithm slower than the stereo algorithm, for which 3 correspondences are sufficent to estimate parameters.

-----

[TOC]

# Prerequisites

* `sudo apt-get install libpng++-dev`

# Build & Run

## Compiling Matlab Wrappers

* In the MATLAB directory of libviso, simply run **make.m** to generate **the mex wrappers** (Run `mex -setup` before to choose your desired compiler)

* Now try to run the **demo\*.m** files (For some of them you will need the **Karlsruhe dataset** from www.cvlibs.net)

## Build C++ Code

* plain cmake (No ROS)
  - `cd viso2_cg/viso2 & mkdir build & cd build & cmake .. -DBUILD_DEMO=true & make`
  - Run `./viso2 path/to/sequence/2010_03_09_drive_0019`

* With ROS
  - `catkin_make` or `catkin build viso2_cg`
  - `roslaunch viso2_cg demo_stereo.launch [rviz:=true]`

# Coordinate System Definition

<div align=center>
  <img src="http://www.cvlibs.net/software/libviso/coordinates.jpg">
</div>

# Dataset

* [Karlsruhe Dataset](http://www.cvlibs.net/datasets/karlsruhe_sequences/): Stereo Video Sequences + rough GPS Poses
* [viso2 sample bagfiles](http://srv.uib.es/public/viso2_ros/sample_bagfiles/)

# Papers

```bibtex
@INPROCEEDINGS{Geiger11,
 author = {Andreas Geiger and Julius Ziegler and Christoph Stiller},
 title = {StereoScan: Dense 3d Reconstruction in Real-time},
 booktitle = {IEEE Intelligent Vehicles Symposium},
 year = {2011},
 month = {June},
 address = {Baden-Baden, Germany}
}
```
