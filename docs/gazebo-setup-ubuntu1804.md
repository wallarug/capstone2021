# Gazebo Install

These are the instructions for setting up Gazebo, ArduPilot (ArduPilot-Gazebo Plugin) and QGroundControl for the drone sims.

Gazebo is the drone/robotics simulator, ArduPilot is the autopilot software, the plugin is used for both of these software to communicate.
QGroundControl is **optional** but is useful for manually setting flight missions and testing whether your setup is working.

For the bashrc related changes, make sure you actually add it in and reload the bashrc file by sourcing it. This will permanently set ENV variables that make Gazebo and ArduPilot work.

## Environment
Ubuntu 18.04


## Pre-reqs
Firstly, make sure you have git:

```bash
sudo apt install git
```
## ArduPilot ##

Next, we will begin by installing ArduPilot:

```bash

git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
git checkout Copter-3.6
git submodule update --init --recursive
```

Install dependencies:

```bash

sudo apt install python-matplotlib python-serial python-wxgtk3.0 python-wxtools python-lxml python-scipy python-opencv ccache gawk python-pip python-pexpect

sudo pip install future pymavlink MAVProxy

```

After that, add these to your ~/.bashrc (**for this to be permanent, don't run on cmd line, actually edit the bashrc file**):

```bash

export PATH=$PATH:$HOME/ardupilot/Tools/autotest
export PATH=/usr/lib/ccache:$PATH

```

Reload the bashrc:

```bash

. ~/.bashrc

```

Test out ArduPilot and build it:

```bash

cd ~/ardupilot
./waf configure --board sitl
./waf copter

cd ArduPilot
sim_vehicle.py -w
```

## Gazebo ##

Set up to accept software from the OSR Foundation:

```bash

sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'

wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -

sudo apt update

```

Install Gazebo:

```bash

sudo apt install gazebo9 libgazebo9-dev

```

## Gazebo ArduPilot Plugin ##

```bash

cd ~
git clone https://github.com/khancyr/ardupilot_gazebo.git
cd ardupilot_gazebo
git checkout dev
mkdir build
cd build
cmake ..
make -j4
sudo make install

```

Append to your bashrc:

```bash

source /usr/share/gazebo/setup.sh

export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models

```

Reload bashrc:

```bash

. ~/.bashrc

```

Test if it works:

```bash

gazebo --verbose ~/ardupilot_gazebo/worlds/iris_arducopter_runway.world

### On a separate terminal window, run below:
cd ~/ardupilot/ArduCopter/
sim_vehicle.py -v ArduCopter -f gazebo-iris --console

```

Gazebo should run from the first command, the second command should initialize two windows - one white and one black.

## QGroundControl (Optional) ##


```bash

sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager

wget https://s3-us-west-2.amazonaws.com/qgroundcontrol/latest/QGroundControl.AppImage

chmod +x ./QGroundControl.AppImage 
./QGroundControl.AppImage  

# Test it out
cd ~/ardupilot/ArduCopter/
sim_vehicle.py

```

Credit:[Intelligent Quads](https://www.youtube.com/channel/UCuZy0c-uvSJglnZfQC0-uaQ)


Below are the guides I used:

[ArduPilot](https://github.com/Intelligent-Quads/iq_tutorials/blob/master/docs/Installing_Ardupilot.md)

[Gazebo and the ArduPilot-Gazebo Plugin](https://github.com/Intelligent-Quads/iq_tutorials/blob/master/docs/installing_gazebo_arduplugin.md)

[QGroundControl](https://github.com/Intelligent-Quads/iq_tutorials/blob/master/docs/installing_qgc.md)
