# rosbot-navigation

Setting target point in RViz (running on your PC) for ROSbot driving autonomously thanks to Nav2 stack. Works both in the LAN network and [over the Internet](https://husarion.com/manuals/rosbot/remote-access/). 

## PC

Clone this repository:

```
git clone https://github.com/husarion/rosbot-navigation.git
```

Create a map of the environment using the [rosbot-mapping](https://github.com/husarion/rosbot-mapping) project template.

Copy `rosbot-mapping/maps` folder to `rosbot-navigation/maps` (the same directory as this README).

Check your configs in `.env` file

```
LIDAR_SERIAL=/dev/ttyUSB0

# for RPLIDAR A2M8
# LIDAR_BAUDRATE=115200
# for RPLIDAR A2M12 and A3
LIDAR_BAUDRATE=256000

DDS_CONFIG=DEFAULT
# DDS_CONFIG=HUSARNET_SIMPLE_AUTO

RMW_IMPLEMENTATION=rmw_fastrtps_cpp
# RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
# on ROSbot run "export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp" in the terminal (system env has precedence over .env file)

```

**Notes:**
- Usually RPLIDAR is listed under `/dev/ttyUSB0`, but verify it with `ls -la /dev/ttyUSB*` command.
- If you have RPLIDAR A3 or A2M12 (with violet border around the lenses) set: `LIDAR_BAUDRATE=256000`. Otherwise (for older A2 LIDARs): `LIDAR_BAUDRATE=115200`.
- With `DDS_CONFIG=DEFAULT` your robot and laptop need to be in the same LAN network. If you want to use this demo over the Internet, set `DDS_CONFIG=HUSARNET_SIMPLE_AUTO` and [enable Husarnet on ROSbot and you PC](https://husarion.com/manuals/rosbot/remote-access/).


To sync workspace with ROSbot execute (in `rosbot-navigation` directory):

```bash
./sync_with_rosbot.sh <ROSbot_ip>
```

Run RViz and set the initial `2D Pose Estimate` by using the button in RViz UI:

```bash
xhost +local:docker && \
docker compose -f compose.pc.yaml up
```

Now, navigate around with the `2D Goal Pose` button in RViz UI.

## ROSbot

> **Firmware version**
>
> Before running the project, make sure you have the correct version of a firmware flashed on your robot.
>
> Firmware flashing command (run in the ROSbot's terminal)
>
> ```
> docker stop rosbot microros || true && docker run \
> --rm -it --privileged \
> husarion/rosbot:humble-22-12-08 \
> /flash-firmware.py /root/firmware.bin
> ```

In the ROSbot's shell execute (in `/home/husarion/rosbot-navigation` directory):

```bash
docker compose -f compose.rosbot.yaml up
```