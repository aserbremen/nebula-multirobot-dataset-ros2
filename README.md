# NeBula Multi-Robot Processor ROS2

Playback and analyze the [NeBula multi-robot dataset](https://github.com/NeBula-Autonomy/nebula-multirobot-dataset) with `nebula_multirobot_processor.py` in ROS2. Tested in ROS2 Humble.

## Dataset structure and preparation

- Download the [dataset](https://github.com/NeBula-Autonomy/nebula-multirobot-dataset/blob/main/dataset.md) and extract it. 
- Convert `robot.bag` in the `groundtruth` folder and `robot_odom.bag` in the `rosbag` ROS2 using the [rosbags](https://pypi.org/project/rosbags/) python package. 

<code> rosbags-convert husky4_odom.bag # for all bag files </code>

You end up with the following folder structure, e.g. `urban` dataset. ROS2 folders and files are marked with a `# ROS2` comment:

```bash
.
└── urban
    ├── g2o_pcd
    │   ├── pcd.zip
    │   └── result.g2o
    ├── ground_truth
    │   ├── husky1_odom             # ROS2
    │   │   ├── husky1_odom.db3     # ROS2
    │   │   ├── metadata.yaml       # ROS2
    │   ├── husky1_odom.bag
    │   ├── husky4_odom             # ROS2
    │   │   ├── husky4_odom.db3     # ROS2   
    │   │   ├── metadata.yaml       # ROS2
    │   ├── husky4_odom.bag
    │   ├── result.g2o
    │   ├── spot1_odom              # ROS2
    │   │   ├── metadata.yaml       # ROS2
    │   │   ├── spot1_odom.db3      # ROS2
    │   ├── spot1_odom.bag
    │   └── urban.pcd
    └── rosbag
        ├── husky1                  # ROS2
        │   ├── husky1.db3          # ROS2
        │   └── metadata.yaml       # ROS2
        ├── husky1.bag
        ├── husky4                  # ROS2
        │   ├── husky4.db3          # ROS2
        │   └── metadata.yaml       # ROS2
        ├── husky4.bag
        ├── spot1                   # ROS2
        │   ├── metadata.yaml       # ROS2
        │   └── spot1.db3           # ROS2
        └── spot1.bag
```

## Usage

### Prerequisites

ROS2 needs to be installed. I tested it with [ROS2 Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html).

For playing back the data you need the `pose_graph_msgs` package from the lamp repository compiled and sourced. You can find a ROS2 version of the package in this repository. Take a look at this [pose_graph_msgs README.md](pose_graph_msgs/README.md) for more information.

Besides the ROS2 python packages you need the following python packages:

- [rosbags](https://pypi.org/project/rosbags/) for converting the rosbags to ROS2
- [python-fire](https://github.com/google/python-fire)
- [matplotlib](https://matplotlib.org/)
- [numpy](https://numpy.org/)
- [pyquaternion](http://kieranwynn.github.io/pyquaternion/)

<code> pip3 install rosbags python-fire matplotlib numpy pyquaternion </code>

### Play back data of any number of robots

The `play_rosbags` function of the `nebula_multirobot_processor.py` script can be used to play back the data of multiple robots in order of their time stamps. It uses the python fire module to execute different functions from the command line. 

The processor class is a ROS2 node which reads in the .db3 files of the robots and publishes the pointcloud and odometry data. The `dataset_dir` parameter needs to be set to the path of the dataset folder [`urban`, `tunnel`, `prelim2`, `ku`]. The `rate` parameter can be used to set the playback rate. The `robot_names` parameter is a list of the robot names which should be played back. 

The point cloud of each keyframe will be published together with the closest odometry message. The topics can be set in the script.

<code> python3 nebula_multirobot_processor.py play_rosbags --ros-args -p dataset_dir:=/path/to/data/dir/urban/ -p rate:=10.0 -p robot_names:="[husky1, husky4]" </code>

### Print dataset info

The `print_info` function of the `nebula_multirobot_processor.py` script can be used to print information about the dataset. The `dataset_dir` parameter needs to be set. 

<code>python3 nebula_multirobot_processor.py print_info --ros-args -p dataset_dir:=/path/to/data/dir/tunnel/ -p robot_names:="[husky3]" </code>

###  Plot 3D trajectories

The `plot_trajectories` function of the `nebula_multirobot_processor.py` script can be used to plot the 3D trajectories of the robots, to get a general idea of the robots' trajectories. 

<code> python3 nebula_multirobot_processor.py plot_trajectories --ros-args -p dataset_dir:=/path/to/data/dir/tunnel/ -p robot_names:="[husky3, husky4]" </code>

### Ground truth pose file creation

The `write_odom_groundtruth function` function of the  `nebula_multirobot_processor.py` script can be used to create ground truth pose files (timestamp x y z qx qy qz qw) for the robots. However for simplicity I added the pose files in the `odom_groundtruth` of this repo. 
