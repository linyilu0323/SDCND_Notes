# Lesson 15: Robot Operating System (ROS)

## Class Notes

## 1. Intro to ROS

- ROS is an open-source, meta-operating system for robot. It provides the framework for commonly used functionality on a robotic system.
- **ROS Nodes** are individual processes that enable some small, basic, relatively specific functions, e.g. sensor read-outs, motor controller, etc. **ROS Master** acts as a manager of all nodes. 
- **Topic** is the messages being shared between nodes, the node sending the topic is called "publisher", and the receiver is called "subscriber", this is referred to as "pub-sub" architecture.
- ROS comes with various pre-defined **message types**, but you can always define your own.
- **ROS Service** is a communication method in request/response scheme, unlike pub-sub, where it's "send whenever available".
- **Compute Graph** is a diagram to help visualize what nodes exist and how they communicate with each other.



### 2. ROS Basic Commands

- Start the master process: `roscore` 
- Run Turtlesim nodes:
  -  `rosrun turtlesim turtlesim_node` <-- this is the main turtlesim node
  -  `rosrun turtlesim tutle_teleop_key` <-- this allows to use arrow keys to control turtle
- List all active nodes: `rosnode list`
- List all topics: `rostopic list`
- Get information about a specific topic: `rostopic info topicxxxxx`
- Show message information: `rosmsg info messagexxx`
- Echo messages on a topic (watch the message in real time): `rostopic echo topicxxxx`



### 3. Writing ROS Nodes

#### 3.1 Simple Mover (Publisher)

```python
#!/usr/bin/env python

import math
import rospy # rospy is the python client library for ROS
from std_msgs.msg import Float64 

def mover():
    # first declare two publishers for joint #1 and #2 respectively
    pub_j1 = rospy.Publisher('/simple_arm/joint_1_position_controller/command',
                             Float64, queue_size=10)
    pub_j2 = rospy.Publisher('/simple_arm/joint_2_position_controller/command',
                             Float64, queue_size=10)
    # initialize client node "arm_mover" and register with the master
    rospy.init_node('arm_mover')
    rate = rospy.Rate(10)
    start_time = 0

    while not start_time:
        start_time = rospy.Time.now().to_sec()

    while not rospy.is_shutdown():
        elapsed = rospy.Time.now().to_sec() - start_time
        pub_j1.publish(math.sin(2*math.pi*0.1*elapsed)*(math.pi/2))
        pub_j2.publish(math.sin(2*math.pi*0.1*elapsed)*(math.pi/2))
        rate.sleep()

if __name__ == '__main__':
    try:
        mover()
    except rospy.ROSInterruptException:
        pass
```

#### 3.2 Arm Mover (Service)

In many respects, `arm_mover` is quite similar to `simple_mover`. Like `simple_mover`, it is responsible for commanding the arm to move. However, instead of simply commanding the arm to follow a predetermined trajectory, the `arm_mover` node provides the service `move_arm`, which allows other nodes in the system to send `movement_commands`. In addition to allowing movements via a service interface, `arm_mover` also allows for configurable minimum and maximum joint angles, by using parameters.

Before the code part, first, we need to create a new service definition.

```python
#!/usr/bin/env python

import math
import rospy
from std_msgs.msg import Float64
from sensor_msgs.msg import JointState
# below: is the user-defined service "simple_arm"
from simple_arm.srv import *

#.......
def mover_service():
    rospy.init_node('arm_mover')
    service = rospy.Service('~safe_move', GoToPosition, handle_safe_move_request)
    rospy.spin()
#.......
```

#### 3.3 Look Away (Subscribers)

```python
#!/usr/bin/env python

import math
import rospy
from sensor_msgs.msg import Image, JointState
from simple_arm.srv import *

# this defines the "LookAway" class and __init__ method
class LookAway(object):
    def __init__(self):
        rospy.init_node('look_away')

        self.sub1 = rospy.Subscriber('/simple_arm/joint_states', 
                                     JointState, self.joint_states_callback)
        self.sub2 = rospy.Subscriber("rgb_camera/image_raw", 
                                     Image, self.look_away_callback)
        self.safe_move = rospy.ServiceProxy('/arm_mover/safe_move', 
                                     GoToPosition)

        self.last_position = None
        self.arm_moving = False

        rospy.spin()
#....
```

