# takeofftheory

## Ardupilot Installation

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

## Running MAVProxy (Ground Control Station) With a Simulated Drone

```
sim_vehicle.py -v ArduCopter -L KSFO --map --console
```

`sim_vehicle.py` simulates a real drone, publishing all telemetry data (airspeed, battery voltage, etc.) to our ground control station. 

The `-v ArduCopter` argument specifies the vehicle, `ArduCopter`, which is a quadcopter. 

The `-L KSFO` argument specifies the drone's spawn location. KSFO is an interesting airport to look at, so we'll use that. 

The `--map --console` arguments open up a map and console to view the drone location and telemetry data.
