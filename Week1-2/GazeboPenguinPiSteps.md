# Under development!

Create a Ubuntu 18 VM

Create virtual environment before installing the packages:
sudo apt-get install python
mkdir YOURENV
python3 -m venv YOURENV
source ./YOURENV/bin/activate
pip install -U pip
pip install flask gevent pyyaml numpy requests opencv-python

Install Gazebo 11: http://gazebosim.org/tutorials?tut=install_ubuntu&ver=5.0
Install ROS Melodic: http://wiki.ros.org/melodic/Installation/Ubuntu
(don't worry about the broken packages warning, it's just because of conflict in Gazebo versions)

Install ROS packages for Gazebo 11: http://gazebosim.org/tutorials/?tut=ros_wrapper_versions
Add the osrfoundation repository to your sources list.
sudo apt install ros-melodic-gazebo11-ros-pkgs 
sudo apt install ros-melodic-gazebo11-ros-control

Use catkin workspaces (melodic) to compile it: http://wiki.ros.org/catkin/Tutorials/create_a_workspace
source /opt/ros/melodic/setup.bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make

Install PenguinPi Gazebo simulation: https://bitbucket.org/cirrusrobotics/penguinpi_gazebo/src/master/
cd ~/catkin_ws/src
git clone https://bitbucket.org/cirrusrobotics/penguinpi_description.git
git clone https://bitbucket.org/cirrusrobotics/penguinpi_gazebo.git
cd ~/catkin_ws
catkin_make
source ~/catkin_ws/devel/setup.bash
roslaunch penguinpi_gazebo penguinpi.launch

In the RViZ window you can visualize the camera feed and the robot's skeleton and other info. In the Gazebo window you can see the robot in the simulated environment (it's quite small so you'll need to zoom in). You can add things in the world. The rays coming from the robot indicates the camera view.

ctrl+c to kill the program

Deactivate the virtual environment after you are done:
deactivate

Run it again after things are installed and machine is restarted:
source ./YOURENV/bin/activate
source ~/catkin_ws/devel/setup.bash
roslaunch penguinpi_gazebo penguinpi.launch

Open a new terminal window, activate the virtual environment again, then run the motor test script to see if the robot in the simulator runs
python3 test_camera_motors.py


Pysical robot
First, switch robot on
Wait for boot... (1 min)
Connect WiFi to the penguinpi:xx:xx:xx (look for it in the WiFi window)
Password: edb439123

ssh -X pi@192.168.50.x
Password: PenguinPi
