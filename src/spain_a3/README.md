# spain_a3
### Allen Spain
### 11/15/2018
Use [hector_slam](http://wiki.ros.org/hector_slam) for occupancy grid mappping using data collected using a [HOKUYO(URG-04LX-UG01)](https://www.hokuyo-aut.jp/search/single.php?serial=166) laser range finder on a Turtlebot. [Rapidly Exploring Random Tree RRT*   algorithm](http://paper.ijcsns.org/07_book/201610/20161004.pdf) was used for planning a path towards a goal determined by a service provider.

_Disclaimer:_ Due to persistent errors under time a time constraint I was unable to synchronize my code (see Reflections section for more details) in the way that I wanted. So this code comes in three parts.
1) Implement hector mapping, for SLAM and to build a map using sensor data.
2) Implement Rapidly exploring Random Tree (RRT) which navigates a predetermined map and navigates towards a goal posted by the service provider.


## Getting Started
1) download ```spain_a3```, ```rrt_exploration_tutorials``` at [https://github.com/hasauino/rrt_exploration_tutorials](https://github.com/hasauino/rrt_exploration_tutorials) and ```hector_slam``` at [https://github.com/tu-darmstadt-ros-pkg/hector_slam](https://github.com/tu-darmstadt-ros-pkg/hector_slam)
2)
3) place in ```[catkin_ws]/src``` folder.
4) run catkin_make, followed by ```source devel setup.bash``` to build all packages within your ```catkin_ws``` and make your ```catkin_make``` visible.

## Running
**For part I (Hector SLAM):**
  2) run: ```roslaunch spain_a3 my_hector_slam_turtlebot.launch```
3) run: ```rosbag play rosbags /assignment3_5floor.bag --clock```
To save the maps for safe keeping in the ```maps``` directory you can run: ```rosrun map_server map_saver -f ~/map/<map_name>```
3) End processes with cmd + C, and ```killall gzerver```
**Part II (RRT and goal pursuit):**
1) run: ```roslaunch spain_a3 find_goal_in_house.launch```
2) To see the robot use RRT to navigate towards the default goal run: ```python fetch_goal.py```

## Testing
Once launched you can set the goal by modifying the position array in ```__main__``` the default position is:
```position = {'x': 1.36, 'y' : -1.46}```


## My Simple Server Summary
My server communicates with action```robot_1/move_base```
It is also necessary to use frame id ```robot_1/map``` which corresponds to my map coordinate frame. This can be examined by running ```tf echo robot_1/map robot_1/move_base``` which will give you the transformation from the map frame to the move base frame.

This was sourced from: [ROS](http://wiki.ros.org/navigation/Tutorials/SendingSimpleGoals)


-----------------------------------
## Reflections
This assignment took much longer than expected, I seem to have run into every error possible, but learned alot in the way of tf, nodes, launch files, gmapping .. etc. I spent alot of time trying to implement an RRT algorithm that gave me errors that could be fixed by changing increases the transform tolerance in the global costmap. I was able to get rid of all intial errors, with the exception of one recurring issue which seemed to be related to memory space. Once the 2D nav pose was published to the goal topic, the program would crash due to an boundary violation this had something to do with the difference between my local costmap, and the one being referenced (global costmap) my map size when compared to some other map ?? Still not sure what the problem was. I spent a considerable abont of time trying to sync the turtlbot + my maps + rrt_exploration but still needs some work.

I was not able to sync up my map with the rrt_exploration algorithm I would really like some feedback here. I attempted to simply add my map path to the existing tutorial, but didn't have any luck. I also wanted to run without gazebo, but despite there not being errors the rrt program didn't run. With minor feedack I would have been able to get everything running in tandum. As a consequency this assignment has two parts. Part I:


## References
* RRT Algorithm: [github.com/hasauino](https://github.com/hasauino)
* Paper on RRT from [Hassan Umari et al.](https://ieeexplore.ieee.org/document/8202319)
* [Turtlebot Tutorial](http://learn.turtlebot.com)
