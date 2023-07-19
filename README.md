# ROS2 Smart Lift

This project aims to develop a physical add-on kit for an elevator to allow the elevator to be controlled by a robot, thereby allowing a single robot to access multiple floors in a building. There are many usecases for such a device, for example in hospitals, hotels, multi-storey garages etc.

The add-on kit will be programmed using ROS2 and is ready for future integration into Robotics Middleware Framework (RMF). Unique QR codes indicating the floor level will be pasted on each floor outside the lift, and a camera inside the lift would be able to detect which floor the elevator is on when the doors open. Servos are actuated to depress corresponding buttons in the lifts (like a human would) to command the elevator to different floors. The add-on kit will communicate with the robot via ROS2 topics over LAN, hence implying a need for wireless access points within the elevator.

### Hardware:
- Raspberry Pi 4
- Raspberry Pi Camera V2.1
- PCA 9685 16 x 12-bit PWM
- SG91R Micro Servo
- 5V Power Source

### Software:
- Ubuntu 22.04
- ROS2 Humble


### Custom Interfaces:
Custom interfaces were developed to communicate between a robot and this Raspberry Pi add-on kit via DDS. The lift_interfaces folder will need to be placed in the src folder in a ROS2 workspace.
The custom interface includes:

**LiftRequests** - To be published by the robot on the ```/lift_requests``` topic

**LiftStates** - To be subscribed by the robot on the ```/lift_states``` topic


## How to use this:
To start the application, there are a few ROS2 nodes that have to be run: Most of the code is written in the lift_controller_v2 package, which will be in the src folder in the ROS2 workspace. The ```image_tools``` package should already be built into the ROS2 Humble desktop installation.

The ```lift_control_node``` subscribes to the ```/lift_requests``` topic and recieves the request from the robot.
```
ros2 run lift_controller_v2 lift_control_node
```

The ```button_press_node``` actuates the correct servos at the correct timings, using information about the current floor that the elevator is on, and the lift request.
```
ros2 run lift_controller_v2 button_press_node
```

The ```qr_decoder``` node analyzes the images from the Raspberry Pi camera and decodes the QR codes in the image indicating the floor that the elevator is currently on.
```
ros2 run lift_controller_v2 qr_decoder
```

The ```cam2image``` node publishes the images taken by the Raspberry Pi camera for decoding.
```
ros2 run image_tools cam2image
```
