baxter_simulator
=============
RSDK Wiki: http://github.com/RethinkRobotics/sdk-docs/wiki

## Repository Description
Necessary files for the Gazebo™ simulation of the Baxter Research Robot from Rethink Robotics Inc.


## Prequisites

 * Setup Github - the git@github.com urls, below, only work if you 
   have [Setup Github](https://help.github.com/articles/set-up-git) and 
   generated [SSH Keys for Github](https://help.github.com/articles/generating-ssh-keys).

## Prequisites

```
$ sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu precise main" > /etc/apt/sources.list.d/gazebo-latest.list'
$ wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install python-wstool python-rosdep ros-groovy-pcl-conversions ros-groovy-control-msgs ros-groovy-cmake-modules ros-groovy-moveit-full ros-groovy-driver-common ros-groovy-image-common ros-groovy-rostest gazebo

```

## Baxter Installation

* Create a catkin workspace and cd into it:

```
    mkdir -p ~/catkin_ws/src
    cd ~/catkin_ws/src
    catkin_init_workspace
    cd ..
    catkin_make

```

* Copy rosinstall file:

```
    $ wget (ftp address of rosinstall file) - will add later
(for now copy in the attached file)

```

* Run wstool:

```
    $ wstool init -j8 src baxter_simulation.rosinstall

```

* Build:

```
    $ source /opt/ros/groovy/setup.bash
    $ catkin_make
    $ catkin_make install 

```

* Use baxter.sh - it has a special hook for sim:

'''
    $ cp src/baxter/baxter.sh .
'''

#Edit the your_ip value in baxter.sh

* Run the script with sim specified:

'''
    $ baxter.sh sim
'''


* Known Issues

   If you get the following error:

   ERROR: the following packages/stacks could not have their rosdep keys resolved to system dependencies:
   joint_trajectory_controller: Cannot locate rosdep definition for [xacro]

You should build xacro from source using the hydro branch:

```
    cd ~/catkin_ws/src
    git clone https://github.com/ros/xacro.git -b hydro-devel
```

### Simulation 

 * Start simulation with controllers:
   ```
   roslaunch baxter_gazebo baxter_world.launch
   ```
   By default the position controllers are started. To switch, use the JointCommand topic 
   as documented in the Baxter SDK.

 * Optional: Test/tune the velocity controllers or position controllers using a RQT dashboard GUI. 
   Make sure you are in the right joint command mode when using these:

   ```
   roslaunch baxter_control baxter_sdk_position_rqt.launch
   ```
   or
   ```
   roslaunch baxter_control baxter_sdk_velocity_rqt.launch 
   ```

## Start MoveIt

Works with simulation or hardware:


 * Start Trajectory Controller:

   ```
   rosrun baxter_interface joint_trajectory_action_server.py
   ```

 * Start MoveIt:

   ```
   roslaunch baxter_moveit_config baxter_bringup.launch
   ```

## Run SDK Examples

 * Start Wobbler example:

   ```
   source ~/catkin_ws/devel/setup.sh
   ~/catkin_ws/src/baxter_simulator/baxter_spoof.sh
   rosrun joint_velocity wobbler.py
   ```

 * Start keyboard joint position example:

   ```
   source ~/catkin_ws/devel/setup.sh
   ~/catkin_ws/src/baxter_simulator/baxter_spoof.sh
   rosrun joint_position keyboard.py

   ```


