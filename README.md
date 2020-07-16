# Calibration for one LIDAR and one camera

This code is reffered from the old version of Autoware, and only extract the calibration part.

### Setup

This code has been tested in the environment of Ubuntu18.04, ROS Melodic. For other versions of ROS, it should also work theoretically. 

- ROS should be installed firstly. The install tutorial can reffer `https://www.ros.org/install/`

- Code compile

  Firstly, new a directory called `/catkin_usv` in your `/home/${YOUR NAME}` and add a new directory `/src` in  `/home/${YOUR NAME}/catkin_usv` . Then extract our repository and put it into `/home/${YOUR NAME}/catkin_usv/src`.

  ```bash
  $cd /home/${YOUR NAME}/catkin_usv
  $catkin_make
  ```

### Note: the error may happen when compile using `catkin_make`.

- 1. autoware_msgs包报错

   ```bash
  Could NOT find jsk_recognition_msgs (missing: jsk_recognition_msgs_DIR)
   ```

  **解决：可能是安装ros包的时候没装完整。**

  ```bash
  $ sudo apt install ros-melodic-jsk-recognition-msgs
  ```
  
- 2. `caktin_make`的make阶段报错

   ```bash
  fatal error: nlopt.hpp: No such file or directory
   ```

  **解决：**

  ```bash
  $ sudo apt-get install libnlopt-dev 
  ```

### Usage

- Preparation

  ​	We should prepare a chessboard which is 8X6 for pattern number and 0.108mX0.108m for pattern size. Other number and size also be alright. But **Note**: sparser your point cloud is, bigger the chessboard shold be. For 16-lines LIDAR, the pattern size can be about 0.2mX0.2m.

- Calibration ROSBAG collection

  ​	According to the official document: `./doc/CalibrationToolkit_Manual.pdf`, we set 9 poisitons and 3 configurations with different tilt angles of the plane. Then record it.
  
  ```bash
  $rosbag record -O my_calibration_dataset ${Your Image Topic} ${Your Pointcloud Topic}
  ```

- Calibrate

  Firstly, you should play the ROSBAG file `my_calibration_dataset.bag`.

  ```bash
  $rosbag play --pause -l my_calibration_dataset.bag
  ```

  Then, run the calibration toolkit.

  ```bash
  $cd /home/${YOUR NAME}/catkin_usv
  $source devel/setup.bash
  $cd devel/lib/calibration_camera_lidar
  $./calibration_toolkit
  ```

### Quick Key

- Translation : ↑, ↓, ←, →, PgUp, PgDn;
- Rotation : a, d, w, s, q, e;
- Projection mode switch : 1 for perspective projection, 2 for orthogonal projection;
- In orthogonal projection mode : - or , for small viewport, + or . for large viewport;
- Point size : o for small, p for large;
- Line width : k for narrow, l for broad;
- Change background color : b for color selection;
- Change light color : n for color selection;
- Clear screen : Delete.

