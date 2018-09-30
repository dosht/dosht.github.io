---
layout: post
title:  "Crazyflie Hello World!"
author: dosht
categories: [ Crazyflie, Drones ]
image: assets/images/Crazyflie2.0-hello-wrold.jpg
featured: true
hidden: true
---
This post is using [Crazyflie 2.0](https://www.bitcraze.io/crazyflie-2/).
It's a kick start to build a small drone and control it autonmously from the computer,
so you will need also [Crazyradio PA](https://www.bitcraze.io/crazyradio-pa/) to connect your python program to the drone through.
You will need also at least [`python-pip`](https://pip.pypa.io/en/stable/) and [`git`](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

# Building and configuring the drone
### 1. Build the drone
First thing after getting the drone in a box, you will need to build it, so you can follow [walk through](https://www.bitcraze.io/getting-started-with-the-crazyflie-2-0/) to assemble the hardware.

### 2. Install the client (and the driver)
Then, install the [client](https://www.bitcraze.io/download/) on your computer (and the [driver](https://wiki.bitcraze.io/doc:crazyradio:install_windows_zadig) if you're using windows). This will be helpful to test your connection, update the firmware and change the configuration.

(Optional) You can install the [android](https://play.google.com/store/apps/details?id=se.bitcraze.crazyfliecontrol2) or the [iOS](https://itunes.apple.com/us/app/crazyflie-2.0/id946151480) client and fly it manually. You can also use the mobile client to upgrade it's firmware.

### 3. Configure upgrade the firmware
Final setup, open the client and update the firmware [here] or using the mobile client.(https://www.bitcraze.io/getting-started-with-the-crazyflie-2-0/#update-fw).
 Then navigate to `connect`, select the `configure 2.0` option and change the bandwidth from `250k` to `2M`.

# Getting started
Clone this [repo](https://github.com/mikecentral/Crazyflie-Drone).

```bash
git clone https://github.com/mikecentral/Crazyflie-Drone
```

Then install cflib using python pip as follows:

```bash
pip install cflib
```

# Run the program
Now we will run `autonomousSequence.py`, but before you run the code, you need to make sure you have enough space in your room to fly the drone in 1.5 meters in each direction, or you can set `SIDE` constant to a smaller value, e.g. `0.5`.

Another practical tip: You can add the emergency landing callback.
That will send a landing command to the drone if you decided to exit the program immediatly for any reason.
Add this code to the `main` function:

```python
import sys
import signal

def signal_handler(sig, frame):
    print(f"EMERGENCY LANDING BY USER!")
    scf.cf.commander.send_setpoint(0, 0, 0, 0)
    sys.exit(0)
```

Now you are ready to run the program:

```bash
python autonomousSequence.py
```

# First look at the code