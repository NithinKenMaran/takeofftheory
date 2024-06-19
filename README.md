# takeofftheory

## Installation: Ardupilot

Install git:
```
sudo apt install git
```
Clone the Ardupilot repository:
```
cd ~
git clone https://github.com/ArduPilot/ardupilot.git
cd ardupilot
```
Install some dependencies:
```
Tools/environment_install/install-prereqs-ubuntu.sh -y
```
This completes the installation; close this terminal and open a new terminal. 

## Running Ground Control Station With a Simulated Drone

```
sim_vehicle.py -v ArduCopter -L KSFO --map --console
```

`sim_vehicle.py` simulates a real drone, publishing all telemetry data (airspeed, battery voltage, etc.) to our ground control station. 

The `-v ArduCopter` argument specifies the vehicle, `ArduCopter`, which is a quadcopter. 

The `-L KSFO` argument specifies the drone's spawn location. KSFO is an interesting airport to look at, so we'll use that. 

The `--map --console` arguments open up a map (with the drone) and a console (to monitor telemetry data like airspeed and altitude).

### Documentation
If you're interested in building your own drone in the future, I strongly recommend looking at Ardupilot's documentation on using the simulation: https://ardupilot.org/dev/docs/using-sitl-for-ardupilot-testing.html

Look at simulating different vehicles, like planes and rovers, with the `-v` argument. 

# Installation: Gazebo Simulator

```
sudo apt-get update
sudo apt-get install lsb-release wget gnupg
sudo wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null

sudo apt-get update
sudo apt-get install gz-garden

sudo apt install libgz-sim7-dev rapidjson-dev
```

### Installation: Ardupilot plugin for gazebo
```
cd ~
mkdir -p gz_ws/src && cd gz_ws/src
git clone https://github.com/ArduPilot/ardupilot_gazebo
export GZ_VERSION=garden
cd ardupilot_gazebo
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j4

echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/gz_ws/src/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
echo 'export GZ_SIM_RESOURCE_PATH=$HOME/gz_ws/src/ardupilot_gazebo/models:$HOME/gz_ws/src/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc

source ~/.bashrc
```

## Run 3D-Simulated Drone with Ardupilot ground control station

In a new terminal, run this command:
```
gz sim -v4 -r iris_runway.sdf
```

This should open a drone kept on a runway, in gazebo.

In another terminal window, run this command to start Ardupilot ground control station:
```
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console
```

## Terminal commands to control the drone

### Takeoff sequence:

```
mode guided
arm throttle
takeoff 10
```
This will make the drone takeoff to a height of `10` metres.

### Fly to a specific position:
```
position N E D
```
Here, position argument is in NED (North-East-Down coordinates). 

For example, `position 10 5 2` makes the drone fly 10 metres forward, 5 metres to the right, and 2 metres down.

### Setting velocity:
```
velocity X Y Z
```
Here, X Y Z are the axes in the drone's frame of reference. 


### Set target speed in automated flight:
```
setspeed N
```

For example, after executing `position 10 0 0` (to move 10 metres forward), executing `setspeed 5` will make the drone move forward 10 m, at a speed of 5 m/s.

### Control Yaw (rotation about vertical axis):
```
setyaw <ANGLE> <ANGULAR_SPEED> <ANGLE_MODE>
```

Here, `ANGLE_MODE` can either be 0 (absolute) or 1 (relative), to set the frame of reference  with respect to which the rotation should be done. 
