# ROBT-403-Laboratory-3
ROBT 403 Laboratory 3
# Laboratory 3: Joint-Control-by-ROS-Gazebo

## PART 1

### Note: This project is about creating a rosnode that is designed for "listening" std_msgs/Float64 type data and “publishing” it  to the joint of the planar robot. The node should send the command to move if the any new incoming value is lower than the previous one.

## Usage

### Installing packages and etc.:
```command
sudo apt-get install ros-kinetic-ros-control ros-kinetic-ros-controllers
```
```
git clone https://github.com/arebgun/dynamixel_motor
catkin_make
source ~/CATKIN_WORKSPACE/devel/setup.bash
```
```
git clone https://github.com/fenixkz/ros_snake_robot.git
catkin_make
```
```
source ~/CATKIN_WORKSPACE/devel/setup.bash
```
```
roslaunch gazebo_robot gazebo.launch
```
## Result:

https://user-images.githubusercontent.com/47817099/143200814-49a094da-d362-46e8-a588-b2891b715904.mp4

```cpp
    ros::NodeHandle nh;
    //for second joint Publisher name would be pub2 and for Subscriber sub2
    ros::Publisher pub1 = nh.advertise<std_msgs::Float64>("/robot/joint1_position_controller/command", 100);
    ros::Subscriber sub1 = nh.subscribe("Aigerim_joint1_angle", 1, joint1angleCallback);
```
### Get the step response of (you can create a node that will send a square-wave function):

* the joint at the base of the robot
In my case I made a node that sends a square-wave function as below:
```cpp
float X = 0;

void joint1angleCallback(const std_msgs::Float64 msg)
{
    X = msg.data;
	ROS_INFO("X: %f", X);
}

int main(int argc, char **argv){
    float i = 0.0;
    float k; 
    ros::init(argc, argv, "joint1moving");

    ros::NodeHandle nh;

    ros::Publisher pub1 = nh.advertise<std_msgs::Float64>("/robot/joint1_position_controller/command", 10);
     ros::Subscriber sub1 = nh.subscribe("Aigerim_joint1_angle", 10, joint1angleCallback);
    ros::Rate loop_rate(10);
    ros::Time startTime = ros::Time::now();

    while (ros::ok()) {
                 k = sin(i);
                 if (k > 0) {
	                   k = 1;			
                  } else {
                       k = 0;
                  }
                  std_msgs::Float64 msg_to_send;
                  msg_to_send.data = k;
                  pub1.publish(msg_to_send);
                  ROS_INFO("Moving joint 1 to position %f", msg_to_send.data);
                  i = i + 0.1;
                  ros::spinOnce();
                  loop_rate.sleep();
    }
   return 0;
}
```
## PART 2
## Lab 3 part 2 tasks:

1) Get the step response:
1. the joint at the base of the robot
2. the joint at the end-effector of the robot

2) Get the sine-wave response of (you can create a node that will send a sine-wave
function):
3. the joint at the base of the robot
4. the joint at the end-effector of the robot

3) Decrease the Proportional gain of PID in the joints and repeat II and III.
Use rqt (type rqt in terminator, open tab Plugins  Configuration  Dynamic
Reconfigure). From the available configs find robotjointNUMBERPIDP

### RQT: sine wave and step responses:

## Sine-wave-response moving of the planar robot:

https://user-images.githubusercontent.com/47817099/143201461-bf1be376-7b2f-4b2c-9883-066e3d0c5860.mp4

sine-wave and effector:

![image](https://user-images.githubusercontent.com/47817099/143202608-a6b87c87-533b-4836-8821-8230fc6f89d3.png)

joint response:

![image](https://user-images.githubusercontent.com/47817099/143202766-9ad09870-da79-438e-b3e1-d44f8185b036.png)


## Step-response moving of the planar robot:

![image](https://user-images.githubusercontent.com/47817099/143201419-4f021f44-cb48-4273-a2cd-b89894e7cacb.png)

step-signal of the effector:

![image](https://user-images.githubusercontent.com/47817099/143202866-7653387f-575e-4ad2-a9d7-70c01f250855.png)

joint response:

![image](https://user-images.githubusercontent.com/47817099/143202997-6cf64c8a-989d-4df3-a3cf-f46996d884f6.png)


## Thank you for your attention!
