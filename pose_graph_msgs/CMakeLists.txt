cmake_minimum_required(VERSION 3.5)
project(pose_graph_msgs)

set(CMAKE_CXX_STANDARD 14)

find_package(ament_cmake REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)

set(msg_files
  "msg/PoseGraph.msg"
  "msg/PoseGraphNode.msg"
  "msg/PoseGraphEdge.msg"
  "msg/PoseAndScan.msg"
  "msg/KeyedScan.msg"
  "msg/KeyValue.msg"
  "msg/LoopCandidate.msg"
  "msg/LoopCandidateArray.msg"
  "msg/LoopComputationStatus.msg"
  "msg/CommNodeInfo.msg"
  "msg/CommNodeStatus.msg"
  "msg/MapInfo.msg"
)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  DEPENDENCIES std_msgs geometry_msgs sensor_msgs
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()
