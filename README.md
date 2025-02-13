# ROS2 Robotics Fundamentals

Welcome to the ROS2 Robotics Fundamentals repository! This is your starting point for learning about ROS2 and robotics in our business.

## Introduction

This repository is designed to help newcomers get started with ROS2 (Robot Operating System 2) and fundamental robotics concepts. Whether you're new to robotics or transitioning from ROS1, you'll find valuable resources here.

## Contents

1. [Installation Guide](#installation-guide)
2. [ROS2 Basics](#ros2-basics)
3. [First ROS2 Project](#first-ros2-project)
4. [Robotics Concepts](#robotics-concepts)
5. [Contributing](#contributing)
6. [Resources](#resources)

## Installation Guide
# Installing ROS 2 (Humble)
## Docker Installation
### Prerequisites
1. Install Docker Desktop
   ```bash
   # For macOS: Download from https://www.docker.com/products/docker-desktop
### Using ROS 2 with Docker and VNC
1. Pull the ROS 2 Desktop VNC image:
   ```bash
   docker pull tiryoh/ros2-desktop-vnc:humble
   ```
2. Run the container:
   ```bash
   docker run -p 6080:80 --shm-size=512m tiryoh/ros2-desktop-vnc:humble
   ```
3. Access the desktop environment:
   - Open your web browser
   - Go to: http://localhost:6080
## ROS2 Basics
[Brief overview of ROS2 concepts like nodes, topics, services, etc.]
# Configuring Your ROS 2 Workspace
A ROS 2 workspace is a directory containing ROS 2 packages. This guide explains how to set up and manage your workspace.
## Creating a New Workspace
1. Create workspace directory:
   ```bash
   mkdir -p ~/ros2_ws/src
   cd ~/ros2_ws
   ```
2. Build the workspace (even if empty):
   ```bash
   colcon build
   ```
This creates the following directory structure:
```
ros2_ws/
├── build/
├── install/
├── log/
└── src/
```
## Understanding Workspace Structure
- `src/`: Source space - Contains source code of ROS 2 packages
- `build/`: Build space - Intermediate files during package building
- `install/`: Install space - Built packages and executables
- `log/`: Log files from build and various tools
## Sourcing the Workspace
After building, source the workspace:
```bash
source ~/ros2_ws/install/setup.bash
```
Add to your `.bashrc` for permanent sourcing:
```bash
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```
## Working with Multiple Workspaces

### Workspace Overlaying
ROS 2 uses workspace overlaying, where the most recently sourced workspace takes precedence:
1. Source ROS 2 installation:
   ```bash
   source /opt/ros/humble/setup.bash
   ```
2. Source your workspace:
   ```bash
   source ~/ros2_ws/install/setup.bash
   ```
### Checking Current Workspace
View current workspace packages:
```bash
ros2 pkg list
```
## Best Practices
1. Keep workspaces organized:
   - One workspace for development
   - Separate workspace for experiments
   - Don't mix different ROS 2 distributions
2. Use meaningful workspace names:
   ```bash
   ~/ros2_main_ws/    # Main development
   ~/ros2_test_ws/    # Testing
   ~/ros2_demo_ws/    # Demos
   ```
3. Regular maintenance:
   ```bash
   # Clean build files
   rm -rf build/ install/ log/   
   # Rebuild workspace
   colcon build --symlink-install
   ```
## Troubleshooting
1. Package not found:
   - Ensure workspace is sourced
   - Check package location in src/
   - Rebuild workspace
2. Build errors:
   ```bash
   # Clean and rebuild
   rm -rf build/ install/
   colcon build --symlink-install
   ```
3. Dependencies:
   ```bash
   # Install dependencies
   rosdep install --from-paths src --ignore-src -r -y
   ```

   # Understanding ROS 2 Packages

ROS 2 packages are the main organizational unit for ROS 2 code. This guide explains how to create and manage ROS 2 packages.

## Package Concepts

A ROS 2 package:
- Contains source code, launch files, parameters, and dependencies
- Must have at least `package.xml` and `CMakeLists.txt` (for C++) or `setup.py` (for Python)
- Should perform a specific function (single responsibility principle)

## Creating a New Package

### Python Package
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_python my_package --dependencies rclpy
```

### C++ Package
```bash
cd ~/ros2_ws/src
ros2 pkg create --build-type ament_cmake my_cpp_package --dependencies rclcpp
```

## Package Structure

### Python Package Structure
```
my_package/
├── my_package/
│   ├── __init__.py
│   └── my_node.py
├── test/
├── package.xml
├── setup.py
└── setup.cfg
```

### C++ Package Structure
```
my_cpp_package/
├── include/my_cpp_package/
├── src/
├── test/
├── CMakeLists.txt
└── package.xml
```

## Package.xml

Essential package metadata file:
```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>my_package</name>
  <version>0.0.1</version>
  <description>My first ROS 2 package</description>
  <maintainer email="you@email.com">Your Name</maintainer>
  <license>Apache License 2.0</license>

  <depend>rclpy</depend>
  
  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>

  <export>
    <build_type>ament_python</build_type>
  </export>
</package>
```

## Common Package Dependencies

### Python Packages
- `rclpy`: ROS 2 Python client library
- `std_msgs`: Standard message types
- `sensor_msgs`: Sensor data messages
- `geometry_msgs`: Geometric messages

### C++ Packages
- `rclcpp`: ROS 2 C++ client library
- `std_msgs`
- `sensor_msgs`
- `geometry_msgs`

## Building Packages

Build all packages in workspace:
```bash
cd ~/ros2_ws
colcon build
```

Build specific package:
```bash
colcon build --packages-select my_package
```

## Running Package Nodes

1. Source workspace:
   ```bash
   source ~/ros2_ws/install/setup.bash
   ```

2. Run a node:
   ```bash
   ros2 run my_package my_node
   ```

## Package Management Commands

```bash
# List all packages
ros2 pkg list

# Package information
ros2 pkg info my_package

# List package executables
ros2 pkg executables my_package

# Install package dependencies
rosdep install --from-paths src --ignore-src -r -y
```

## Best Practices

1. Package Naming:
   - Use lowercase
   - Use underscores for spaces
   - Be descriptive but concise

2. Package Organization:
   - One package per repository for simple projects
   - Multiple related packages in a single repository for complex projects

3. Dependencies:
   - Minimize dependencies
   - Use `package.xml` for all dependencies
   - Keep versions compatible with your ROS 2 distribution



## First ROS2 Project

## First ROS2 Project

This section will guide you through creating your first ROS2 project, covering fundamental concepts of both ROS2 and robotics. By the end of this guide, you'll have a solid foundation for professional robotics development.

### 1. Setting Up Your ROS2 Environment
- Installing ROS2
- Configuring your workspace
- Understanding ROS2 packages

### 2. ROS2 Core Concepts
- Nodes and the ROS2 graph
- Topics and messages
- Services and clients
- Actions
- Parameters
- Launch files
- 

# First ROS2 Project

This section will guide you through creating your first ROS2 project, covering fundamental concepts of both ROS2 and robotics. By the end of this guide, you'll have a solid foundation for professional robotics development.

## 1. Setting Up Your ROS2 Environment

### Installing ROS2
- Follow our detailed guide 
- Ensure your system meets all prerequisites
- Verify installation success

### Configuring your workspace
- Learn about workspace setup 
- Create and build your first workspace
- Understanding workspace structure

### Understanding ROS2 packages
- Package creation and management
- Dependencies and package structure

## 2. ROS2 Core Concepts

### Nodes and the ROS2 graph
- Understanding nodes as fundamental ROS2 computational units
- Node communication and interaction
- Visualizing the ROS2 graph with rqt_graph

### Topics and messages
- Publishing and subscribing to topics
- Message types and custom messages
- Best practices for topic design

### Services and clients
- Request-response communication model
- Creating custom services
- When to use services vs topics

### Actions
- Long-running tasks in ROS2
- Action server and client implementation
- Progress feedback and preemption

### Parameters
- Dynamic configuration in ROS2
- Parameter declaration and usage
- Parameter services and events

### Launch files
- Automating node startup
- Launch file syntax and structure
- Launch file best practices

## Next Steps
After completing this guide, you'll be ready to:
- Create custom ROS2 packages
- Implement basic robotics functionality
- Understand core ROS2 communication patterns
- Build more complex robotics applications

### 3. Creating Your First ROS2 Package
- Package structure
- CMakeLists.txt and package.xml
- Writing a simple publisher and subscriber in Python and C++

### 4. Understanding TF2 (Transform Library)
- Coordinate frames in robotics
- Static and dynamic transforms
- Broadcasting and listening to transforms

### 5. Using URDF (Unified Robot Description Format)
- Creating a simple robot model
- Adding joints and links
- Visualizing your robot in RViz

### 6. Simulating Your Robot
- Introduction to Gazebo
- Launching your robot in simulation
- Adding sensors to your robot (cameras, LiDAR, IMU)

### 7. Robot Control Fundamentals
- Basic control theory
- PID controllers
- Implementing a simple controller in ROS2

### 8. Navigation Stack
- Map creation with SLAM
- Localization techniques
- Path planning and obstacle avoidance

### 9. Computer Vision Basics
- Using OpenCV with ROS2
- Basic image processing
- Object detection and tracking

### 10. Motion Planning
- Configuration space
- Sampling-based planners (RRT, PRM)
- Using MoveIt with ROS2

### 11. ROS2 Best Practices
- Code organization
- Proper use of ROS2 patterns
- Testing your ROS2 code

### 12. Real Robot Integration
- Interfacing with hardware
- Safety considerations
- Calibration techniques

### 13. Advanced Topics
- Multi-robot systems
- Cloud robotics and ROS2
- Machine learning in robotics

### 14. Project: Autonomous Mobile Robot
- Putting it all together
- Building a robot that can autonomously navigate, avoid obstacles, and perform simple tasks

Each of these sections will include code examples, explanations of key concepts, and hands-on exercises to reinforce learning. By working through this project, you'll gain a comprehensive understanding of ROS2 and robotics fundamentals, preparing you for professional work in the field.

## Robotics Concepts

[Introduction to basic robotics concepts relevant to your business]

## Contributing

[Guidelines for how newcomers can contribute to your projects]

## Resources

- [Link to ROS2 documentation]
- [Link to your company's website or additional resources]
- [Any other relevant links]
