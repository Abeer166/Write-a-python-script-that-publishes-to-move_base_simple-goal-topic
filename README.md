# Write-a-python-script-that-publishes-to-move_base_simple-goal-topic
1- create a package

$ cd ~/catkin_ws/src

catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
$ mkdir scripts

$ cd scripts

 write file(node.py)


2- write python script file(node.py)

#!/usr/bin/env python


import rospy

import actionlib

from move_base_msgs.msg import MoveBaseAction, MoveBaseGoal

def movebase_client():

    client = actionlib.SimpleActionClient('move_base',MoveBaseAction)
    
    client.wait_for_server()

    goal = MoveBaseGoal()
    goal.target_pose.header.frame_id = "map"
    goal.target_pose.header.stamp = rospy.Time.now()
    goal.target_pose.pose.position.x = 2.0
    goal.target_pose.pose.orientation.w = 1.0

    client.send_goal(goal)
    wait = client.wait_for_result()
    if not wait:
        rospy.logerr("Action server not available!")
        rospy.signal_shutdown("Action server not available!")
    else:
        return client.get_result()

if __name__ == '__main__':

    try:
    
        rospy.init_node('movebase_client_py')
        
        result = movebase_client()
        
        if result:
        
            rospy.loginfo("Goal execution done!")
            
    except rospy.ROSInterruptException:
    
        rospy.loginfo("Navigation test finished.")
        
  3- change the permission
  
  $ cd catkin_ws/src/beginner_tutorials/scripts
  
  $ sudo chmod +x node.py
  
  4-run the node
  
  $ rosrun beginner_tutorials node.py
  
<img width="1217" alt="Screen Shot 1443-01-01 at 1 58 34 AM" src="https://user-images.githubusercontent.com/56722657/128649626-77f0936f-da98-49f4-a138-f13305b268d6.png">

<img width="390" alt="Screen Shot 1443-01-01 at 2 47 15 AM" src="https://user-images.githubusercontent.com/56722657/128649641-84eb97f8-2b5f-4ec3-bca0-0f2505402504.png">

