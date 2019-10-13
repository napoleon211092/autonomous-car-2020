# RaceCar 2019-2020 - Team: ICT K60

## I. Project Info:

- Team name: teamict

## II. Environment Setup

### 1. Ubuntu 18.04

### 2. Miniconda or Anaconda

### 3. OpenCV 3.4.3

- You can use following script to install OpenCV 3.4.3: <scripts/install_opencv_3.sh>

### 4. Robot Operating System
  
- Install ROS Melodic - full desktop version: <http://wiki.ros.org/melodic/Installation/Ubuntu>
  
### 5. Initialize Catkin workspace

We need to initialize catkin workspace at the first time (build folders for projects).

```terminal
cd main_ws
catkin_make
```

Add workspace to PATH:

```terminal
echo "source !(pwd)/main_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

### 6. Create Conda environment 

```terminal
cd main_ws
conda env create -n cds -f environment.yml 
conda activate cds
```
  
### 7. Dependencies: 

- rosbridge-suite

```terminal
sudo apt-get install ros-melodic-rosbridge-server
```

## III. Build and Run

### Step 1. Build project

```
cd  main_ws
catkin_make
```

## Step 2. Run ROS server and services

**NOTE:** Don't use conda environment here!!!

```
roslaunch teamict server.launch
```

## Step 3. Run the simulator

Enter Team name: `teamict` and server address: `ws://localhost:9090`.

## Step 4. Run car controller

Open another Terminal and type:

```
conda activate cds
roslaunch teamict teamict.launch
```

OR

```
conda activate cds
rosrun teamict teamict_node.py
```

## Step 5. Launch debug image viewer

Because we could not use `cv2.imshow()` to show image in this environment, <main_ws/src/teamict/src/debug_stream.py> is developed to view debug images.

Please use this command to open debug image viewer:

```
rosrun rqt_image_view rqt_image_view
```

## Folder structure and debugging

### Code structure

- Source code for running car (we will mainly work here): <main_ws/src/teamict/src>.
- In that folder:
    + teamict_node.py: main node file
    + config.py: configuration file
    + debug_stream.py: stream of debug images. Please see the usage in teamict_node.py
    + image_processor.py: where to receive images and process.

### How to publish debug images?

- Pass `debug_stream` instance created in `teamict_node.py` to any class you want to debug.
- Create a stream for image using: `debug_stream.create_stream('<stream_name>', '<topic_path>')`

```
debug_stream.create_stream('depth', 'debug/depth')
```

- Publish image: `debug_stream.update_image('<stream_name>', <image>)`

```
debug_stream.update_image('depth', depth_image)
```

- Open debug image viewer:

```
rosrun rqt_image_view rqt_image_view
```
