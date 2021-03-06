# px4_vtol_guide
some useful folders when use standard_vtol mode in pixhawk SITL gazebo simulation

`Ubuntu 16.04 + gazebo7`<br>
## 1.Install tools chain<br>
### (1)Permission<br>
According to PX4 wiki, firstly,we should add user into user group. If not, it may cause many permission problems.But I can build successly without doing this.<br>
   `sudo usermod -a -G dialout $USER`<br>
   `reboot`<br>
  
### (2)Install CMake
   `sudo add-apt-repository ppa:george-edison55/cmake-3.x -y`<br>
   `sudo apt-get update`<br>
  \# necessary software (python、git、qt)<br>
    `sudo apt-get install python-argparse git-core wget zip python-empy qtcreator cmake build-essential genromfs -y`<br>
  \# simulation tools<br>
   `sudo add-apt-repository ppa:openjdk-r/ppa`<br>
   `sudo apt-get update`<br>
   `sudo apt-get install openjdk-8-jre`<br>
   `sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang lldb -y`<br>

### (3)Remove the modemmanager<br>
   `sudo apt-get remove modemmanager`<br>

### (4)Update the following packages<br>
   `sudo add-apt-repository ppa:terry.guo/gcc-arm-embedded -y`<br>
   `sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa`<br>
   `sudo apt-get update`<br>
   `sudo apt-get install python-serial openocd flex bison libncurses5-dev autoconf texinfo build-essential libftdi-dev libtool zlib1g-dev python-empy gcc-arm-none-eabi -y`<br>
Notice:<br>
>>`arm-none-eabi-gcc --version`<br>
>>version 4.9.3 is ok.<br>

### (5)Install git<br>
   `sudo aot-get install git-all`<br>

## 2.Download PX4 firmware<br>
### (1)Download pixhawk firmware from github<br>
   `mkdir -p ~/src`<br>
   `cd ~/src`<br>
   `git clone https://github.com/PX4/Firmware.git`<br>
  
### (2)Update submodule<br>
   `cd Firmware`<br>
   `git submodule update --init --recursive`<br>

### (3)Build px4<br>
   `make px4fmu-v2_default`<br>
Notice: <br>
>>I.v2 --> pixhawk V1; v3 --> pixhawk V2<br>
>>II.When building the newest firmware cloned from github, we faced the problem of "region 'flash' overflowed by xxx bytes". It means that the size of newest code is out of stm32. According to the pix wiki, we should comment unnecessary modules("drivers/trone").But we couldn't find "drivers/trone" in ~/src/Firmware/cmake/configs/nuttx_px4fmu-v2_default.cmake.<br>
  
## 3.Simulation in Gazebo7<br>
Notice:<br>
>>You don't need to install Gazebo7 if you have installed ros-kinetic-desktop-full, because ros-kinetic-desktop-full contains Gazebo7.<br>
>>If you don't have installed ros-kinetic, you should install Gazebo7 separately.<br>
### (1)Install ros-kinetic<br>
  `sudo apt-get install ros-kinetic-desktop-full`<br>
  
### (2)Install gazebo7<br>
   `sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'`<br>
   `wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -`<br>
   `sudo apt-get update`<br>
   `sudo apt-get install gazebo7`<br>
   `sudo apt-get install libgazebo7-dev`<br>
   
### (3)Download models of sun and ground-plane<br>
When firstly launch gazebo, it will download these models.<br>

### (4) Configs for Gazebo
   `sudo apt-get install libprotobuf-dev libprotoc-dev protobuf-compiler libeigen3-dev`<br>
   `cd ~/src/Firmware/Tools/sitl_gazebo`<br>
   `mkdir Build`<br>
\# 设置插件的路径以便 Gazebo 能找到模型文件<br>
   `export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:$HOME/src/Firmware/Tools/sitl_gazebo/Build`<br>
\# 设置模型路径以便 Gazebo 能找到机型<br>
   `export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:$HOME/src/Firmware/Tools/sitl_gazebo/models`<br>
\# 禁用在线模型查找<br>
   `export GAZEBO_MODEL_DATABASE_URI=""`<br>
\# 设置 sitl_gazebo 文件夹的路径<br>
   `export SITL_GAZEBO_PATH=$HOME/src/Firmware/Tools/sitl_gazebo`<br>
   `cd Build`<br>
   `cmake ..`<br>
   `make`<br>

### (5) Test the standard vtol model<br>  
   `cd ~/src/Firmware`<br>
   `make posix_sitl_default gazebo_standard_vtol`<br>
   `commander takeoff`<br>
Notice:
>>quadrotor vehicle: <br>
>>`make posix_sitl_default gazebo`<br>

### (6) Mission mode simulation<br>
Start qgroundcontrol, and the vehicle will connect automatically.<br>
Set waypoints and mission, the vehicle will fly along the path you set.<br> 

## 4.MAVROS simulation
### (1) Install ros-kinetic
   `sudo apt-get install ros-kinetic-desktop-full`<br>
   `sudo rosdep init`<br>
   `rosdep update`<br>
   `echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc`<br>
   `source ~/.bashrc`<br>

### (2) Install MAVROS
   `sudo apt-get install ros-kinetic-mavros*`<br>
   `mkdir -p ~/catkin_ws/src`<br>
   `cd ~/catkin_ws`<br>
### (3) Download offboard package
Download the offboard package which is in our project into ~/catkin_ws/src
   `cd ~/catkin_ws`<br>
   `catkin_make`<br>
   `source devel/setup.bash`<br>
   `echo $ROS_PACKAGE_PATH`<br>
 /home/<youruser>/catkin_ws/src:/opt/ros/kinetic/share:/opt/ros/kinetic/stacks<br>
   

## 5.Add downward camera for the standard vtol model

## 6.Start simulation
