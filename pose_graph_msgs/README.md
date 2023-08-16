# Repo for Nebula autonomy pose graph msgs for ROS2

The `pose_graph_msgs` package contains the ROS2 message definitions for the Nebula autonomy LAMP algorithm. Credit goes to the [LAMP](https://github.com/NeBula-Autonomy/LAMP) repository.

Copy this directory to the `src` folder of your ROS2 workspace and compile it with `colcon build --symlink-install --packages-select pose_graph_msgs`.

Source the workspace with `source install/setup.bash` and you are ready to go.