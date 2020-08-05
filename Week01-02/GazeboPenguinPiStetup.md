# How to set up your simulator environment or robot
This document provides a step-by-step guide on setting up your simulator environment and connecting to the physcial PenguinPi robot.

## Install Simulator Locally (recommended)
![Gazebo 11 inside Ubuntu 18 VM](https://github.com/tianleimin/ECE4078_Lab/blob/master/pics/GazeboVM.png?raw=true "Gazebo 11 inside Ubuntu 18 VM")

### Install by importing my Ubuntu 18 VM image
Install VirtualBox: https://www.virtualbox.org/

Download my Ubuntu VM image: https://drive.google.com/file/d/1Bj8sWKzxKyg0RViiYjnBKiIpHHsbDKMr/view?usp=sharing

Import my Ubuntu VM image to your VirtualBox: https://askubuntu.com/questions/588426/how-to-export-and-import-virtualbox-vm-images

The sudo password is ```meow```

To launch the environment:

```
source ~/catkin_ws/devel/setup.bash
roslaunch penguinpi_gazebo penguinpi.launch
```

In the RViZ window you can visualize the camera feed and the robot's skeleton and other info. 

In the Gazebo window you can see the robot in the simulated environment. It's kind of small so you'll need to zoom in to view it better. For keyboard shortcuts in Gazebo please see [their tutorial](http://gazebosim.org/hotkeys).

Run the test script in your command line to see the robot spinning and taking a photo: ```python3 test_camera_motors.py```

There is a simple web interface for viewing status of the simulated robot, which you can open inside your browser at http://localhost:40000 

### Install from scratch in an empty Ubuntu VM
Create a Ubuntu 18 [Virtual Machine](https://www.virtualbox.org/) if your system isn't Linux: https://releases.ubuntu.com/18.04/

Install required python packages:

```
sudo apt update
sudo apt install python-flask python-gevent
python3 -m pip install --upgrade pip 
python3 -m pip install flask gevent pyyaml numpy requests opencv-python pynput
```

**Note: If you already have ROS Melodic + Gazebo 9 installed, please skip to "Install PenguinPi Gazebo simulation" step. PenguinPi should work with Gazebo 9 just fine.**

Install Gazebo 11: http://gazebosim.org/tutorials?tut=install_ubuntu&ver=11

Install ROS Melodic: http://wiki.ros.org/melodic/Installation/Ubuntu
(in step 1.4 run ```sudo apt install ros-melodic-desktop```)

Install ROS packages for Gazebo 11: http://gazebosim.org/tutorials/?tut=ros_wrapper_versions
(If you haven't done it already when installing Gazebo and ROS: add the osrfoundation repository to your sources list.)

```
sudo apt install ros-melodic-gazebo11-ros-pkgs 
sudo apt install ros-melodic-gazebo11-ros-control
sudo apt install rospkg python-catkin-tools python3-dev python3-catkin-pkg-modules python3-rospkg-modules python3-numpy python3-empy
```

Use catkin workspaces (melodic) to compile the environment: http://wiki.ros.org/catkin/Tutorials/create_a_workspace

```
source /opt/ros/melodic/setup.bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
```

Install PenguinPi Gazebo simulation: https://bitbucket.org/cirrusrobotics/penguinpi_gazebo/src/master/

```
cd ~/catkin_ws/src
git clone https://bitbucket.org/cirrusrobotics/penguinpi_description.git
git clone https://bitbucket.org/cirrusrobotics/penguinpi_gazebo.git
git clone -b melodic https://github.com/ros-perception/vision_opencv.git
cd ~/catkin_ws
catkin_make
```

Lauch the simulation (note that the first time you launch Gazebo it might need a few minutes to download all the models and set things up)

```
source ~/catkin_ws/devel/setup.bash
roslaunch penguinpi_gazebo penguinpi.launch
```

In the RViZ window you can visualize the camera feed and the robot's skeleton and other info. 

In the Gazebo window you can see the robot in the simulated environment. It's kind of small so you'll need to zoom in to view it better. For keyboard shortcuts in Gazebo please see [their tutorial](http://gazebosim.org/hotkeys).

Run the test script in your command line to see the robot spinning and taking a photo: ```python3 test_camera_motors.py```

There is a simple web interface for viewing status of the simulated robot, which you can open inside your browser at http://localhost:40000 

To launch the environment again:

```
source ~/catkin_ws/devel/setup.bash
roslaunch penguinpi_gazebo penguinpi.launch
```

## Cloud-Based Simulator Environments

### ROS Development Studio
**Note: RDS seems to have issues loading texture correctly inside its Gazebo visualisation. It also has lags depending on the internet connection.**

![ROS Development Studio](https://github.com/tianleimin/ECE4078_Lab/blob/master/pics/RDS.png?raw=true "ROS Development Studio")

Get a free account for ROS Development studio: https://rds.theconstructsim.com/

Fork the example project by clicking this link: http://www.rosject.io/l/15e9a94d/

In "Tools -> shell", type the commands below to install required packages:

```
sudo apt update
sudo apt install python-flask python-gevent
python3 -m pip install flask gevent pyyaml numpy requests opencv-python pynput
```

After the packages are installed, in "Simulators -> choose launch file", select "penguinpi.launch" and launch, this will open the Gazebo simulator with a PenguinPi spawn in it (You can dismiss the CPU usage warning if it ever pops up). PenguinPi is sort of small so you'll need to zoom in to view it better. RDS seems to have trouble loading texture correctly inside its Gazebo visualisation so PenguinPi appears pale here, but PenguinPi's own camera feed is not impacted. 

If you want to see more log info, instead of launching the model from "Simulators -> choose launch file", you can open another shell and type

```
source ~/catkin_ws/devel/setup.bash
roslaunch penguinpi_gazebo penguinpi.launch
```

Then open "Tools -> Gazebo", which will again shown the simulator.

Run the Jupyter Notebook cells to see PenguinPi's camera feed and to see it moving inside the simulator.

Open "Tools -> Graphical Tools", this will show you the RViZ and any pop-up windows the python codes launch. **To keyboard teleoperate the robot you'll also need to use keyboard inside the "Graphical Tools".**

Inside "Shell", run the test script ```python3 ./ECE4078/test_camera_motors.py``` 

If you see PenguinPi in the simulator moves and the camera view pops up inside the "Graphical Tools" window then everything is ready for you to start developing the keyboard teleoperation. 

Open "Tools -> IDE", navigate to the ECE4078 folder and start coding away. 

### AWS Remote Desktop
TBA

## Running on the Physical Robot
![PenguinPi Robot](https://github.com/tianleimin/ECE4078_Lab/blob/master/pics/PenguinPi.png?raw=true "PenguinPi Robot")

First, switch the robot on with the side switch

Wait for it to boot... This takes about 1 min. When IP addresses occur on the OLED screen the booting is done.

Connect your PC's wifi to PenguinPi's hotspot penguinpi:xx:xx:xx (look for it in your wifi list). The hotspot password is egb439123

Conect to the robot using ssh (the ssh password is PenguinPi)

```
ssh -X pi@192.168.50.1
```

You can upload files to the robot using the ```scp``` command:
```
scp -r ./LOCALDIR pi@192.168.50.1:~/REMOTEDIR
```

Run the testing script inside the ssh session to see the robot moving and view its camera input using the command: ```python3 test_camera_motors.py```
(Note: the robot inside the simulator uses port number "40000", while the physical robot uses port number "8080". This port number is set in ```simulation_ws/src/scripts/server```, as well as the Notebook and python3 scripts.)

When you are done with the robot, inside the ssh session run ```sudo halt``` to shut down the robot safely. Once its lights stop flashing you can toggle the power switch. Don't toggle the switch directly to "hard" shut it down.

You can connect an external screen, keyboard, and mouse to the robot, then switch it on and use it as a Rasberry Pi. Inside the Rasberry Pi interface, you can install python packages onboard the robot by running pip in the terminal, e.g., ```python3 -m pip install pynput```. You can also install packages inside the ssh session if your PenguinPi has internet connection (you can set the internet connection up in the Rasberry Pi interface).

## Troubleshooting and known issues
The same python scripts should run both in simulator and on the physical robot. The only difference is that the robot inside the simulator uses port number "40000", while the physical robot uses port number "8080".

If you got a connection error trying to view PenguinPi web interface in your local VM, after you have cloned https://bitbucket.org/cirrusrobotics/penguinpi_gazebo/src/master/ to your local VM, inside ```catkin_ws/src/penguinpi_gazebo/scripts```, replace lines 399 to end with the following codes:

```
serverport = 40000
# app.jinja_env.lstrip_blocks = True
# app.jinja_env.trim_blocks = True
# app.jinja_env.line_statement_prefix = '#'
# app.run('0.0.0.0', serverport, debug=True)
http_server = WSGIServer(('', serverport), app)
http_server.serve_forever()
```

To reduce CPU usage, you can reduce the default real-time-update rate in Gazebo from 1kHz to 100Hz by adding the following codes in ```catkin_ws/src/penguinpi_gazebo/worlds/penguinpi.world``` (above the line ```</world>```):

```
    <physics name='default_physics' default='0' type='ode'>
      <max_step_size>0.005</max_step_size>
      <real_time_factor>1</real_time_factor>
      <real_time_update_rate>100</real_time_update_rate>
    </physics>
```

For Python3, ROS, Open-CV compatability issues: https://medium.com/@beta_b0t/how-to-setup-ros-with-python-3-44a69ca36674

The PenguinPi robot can be broken in SO MANY WAYS... So far I have had the wheel connection wire snapping off (need soldering iron to fix), the tail of the frame snapping off during shippment (need to take the whole thing apart and reassamble with a new frame), the camera connection ribbon broken (just get a replacement robot), so please be gentle with it...