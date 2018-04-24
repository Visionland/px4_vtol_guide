# px4_vtol_guide
some useful folders when use standard_vtol mode in pixhawk SITL gazebo simulation

Ubuntu 16.04 + gazebo7
1.Install tools chain
(1)permission
According to PX4 wiki, firstly,we should add user into user group. If not, it may cause many permission problems.But I can build successly without doing this.
  sudo usermod -a -G dialout $USER
  reboot
  
(2)install CMake
  sudo add-apt-repository ppa:george-edison55/cmake-3.x -y
  sudo apt-get update
  # necessary software (python、git、qt)
  sudo apt-get install python-argparse git-core wget zip python-empy qtcreator cmake build-essential genromfs -y
  # simulation tools
  sudo add-apt-repository ppa:openjdk-r/ppa
  sudo apt-get update
  sudo apt-get install openjdk-8-jre
  sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang lldb -y

(3)remove the modemmanager
  sudo apt-get remove modemmanager

(4)update the following packages
  sudo add-apt-repository ppa:terry.guo/gcc-arm-embedded -y
  sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
  sudo apt-get update
  sudo apt-get install python-serial openocd \
    flex bison libncurses5-dev autoconf texinfo build-essential \
    libftdi-dev libtool zlib1g-dev \
    python-empy gcc-arm-none-eabi -y
  Notice:
    arm-none-eabi-gcc --version
    version 4.9.3 is ok.

(5)install git
  sudo aot-get install git-all

2.Download PX4 firmware
(1)download pixhawk firmware from github
  mkdir -p ~/src
  cd ~/src
  git clone https://github.com/PX4/Firmware.git
  
(2)update submodule
  cd Firmware
  git submodule update --init --recursive

(3)build px4
  make px4fmu-v2_default
Notice: 
  I.v2 --> pixhawk V1; v3 --> pixhawk V2
  II.When building the newest firmware cloned from github, we faced the problem of "region 'flash' overflowed by xxx bytes". It means that the size of newest code is out of stm32. According to the pix wiki, we should comment unnecessary modules("drivers/trone").But we couldn't find "drivers/trone" in ~/src/Firmware/cmake/configs/nuttx_px4fmu-v2_default.cmake.
  
3.Simulation in Gazebo7
Notice:
You don't need to install Gazebo7 if you have installed ros-kinetic-desktop-full, because ros-kinetic-desktop-full contains Gazebo7.
If you don't have installed ros-kinetic, you should install Gazebo7 separately.
(1)install ros-kinetic
  sudo apt-get install ros-kinetic-desktop-full
(2)install gazebo7
  sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
  wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
  sudo apt-get update
  sudo apt-get install gazebo7 
  sudo apt-get install libgazebo7-dev
(3)download models of sun and ground-plane
When firstly launch gazebo, it will download these models.
