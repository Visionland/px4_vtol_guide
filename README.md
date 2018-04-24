# px4_vtol_guide
some useful folders when use standard_vtol mode in pixhawk SITL gazebo simulation

`Ubuntu 16.04 + gazebo7`<br>
## 1.Install tools chain<br>
### (1)permission<br>
According to PX4 wiki, firstly,we should add user into user group. If not, it may cause many permission problems.But I can build successly without doing this.<br>
    `sudo usermod -a -G dialout $USER`<br>
    `reboot`<br>
  
### (2)install CMake
    `sudo add-apt-repository ppa:george-edison55/cmake-3.x -y`<br>
    `sudo apt-get update`<br>
  \# necessary software (python、git、qt)<br>
    `sudo apt-get install python-argparse git-core wget zip python-empy qtcreator cmake build-essential genromfs -y`<br>
  \# simulation tools<br>
    `sudo add-apt-repository ppa:openjdk-r/ppa`<br>
    `sudo apt-get update`<br>
    `sudo apt-get install openjdk-8-jre`<br>
    `sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang lldb -y`<br>

### (3)remove the modemmanager<br>
    `sudo apt-get remove modemmanager`<br>

### (4)update the following packages<br>
    `sudo add-apt-repository ppa:terry.guo/gcc-arm-embedded -y`<br>
    `sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa`<br>
    `sudo apt-get update`<br>
    `sudo apt-get install python-serial openocd flex bison libncurses5-dev autoconf texinfo build-essential libftdi-dev libtool zlib1g-dev python-empy gcc-arm-none-eabi -y`<br>
Notice:<br>
>>`arm-none-eabi-gcc --version`<br>
>>version 4.9.3 is ok.<br>

### (5)install git<br>
    `sudo aot-get install git-all`<br>

## 2.Download PX4 firmware<br>
### (1)download pixhawk firmware from github<br>
    `mkdir -p ~/src`<br>
    `cd ~/src`<br>
    `git clone https://github.com/PX4/Firmware.git`<br>
  
### (2)update submodule<br>
    `cd Firmware`<br>
    `git submodule update --init --recursive`<br>

### (3)build px4<br>
    `make px4fmu-v2_default`<br>
Notice: <br>
>>I.v2 --> pixhawk V1; v3 --> pixhawk V2<br>
>>II.When building the newest firmware cloned from github, we faced the problem of "region 'flash' overflowed by xxx bytes". It means that the size of newest code is out of stm32. According to the pix wiki, we should comment unnecessary modules("drivers/trone").But we couldn't find "drivers/trone" in ~/src/Firmware/cmake/configs/nuttx_px4fmu-v2_default.cmake.<br>
  
## 3.Simulation in Gazebo7<br>
Notice:<br>
>>You don't need to install Gazebo7 if you have installed ros-kinetic-desktop-full, because ros-kinetic-desktop-full contains Gazebo7.<br>
>>If you don't have installed ros-kinetic, you should install Gazebo7 separately.<br>
### (1)install ros-kinetic<br>
    `sudo apt-get install ros-kinetic-desktop-full`<br>
### (2)install gazebo7<br>
    `sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'`<br>
    `wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -`<br>
    `sudo apt-get update`<br>
    `sudo apt-get install gazebo7`<br>
    `sudo apt-get install libgazebo7-dev`<br>
### (3)download models of sun and ground-plane<br>
When firstly launch gazebo, it will download these models.<br>
