# Functional description

## Overview

The SF40 is a smart sense-and-avoid LiDAR that detects obstacles near to autonomous vehicles and determines the safest route to
travel past them.

Using a scanning laser to map the surrounding area and identify the presence and position of obstacles, built-in navigation tools
locate gaps and spaces with sufficient clearance for safe navigation. On-board data analysis reduces the need for complicated
processing by an external computer. Hardwired alarm outputs make it easy to interface the SF40 with conventional flight controllers
and a serial port connection allows for greater interaction when navigation becomes more complex.

The SF40 uses a scanning laser rangefinder to measure on a 360 degree disc with a radius of 100 meters. Collected data is stored in
memory and continually refreshed as the laser scans around. The speed of rotation can be set to 1, 2.25 or 4.5 revolutions per
second corresponding to measuring resolutions of 0.03, 0.06 and 0.12 meters respectively. Time critical functions are managed by an
FPGA leaving the 32-bit Arm Cortex-M3 processor to handle the data analysis and monitor system performance and reliability.

!> The SF40 is designed to collect and analyze data internally. Whilst this data can be downloaded to an external controller,
the SF40 is not intended to provide a continuous data stream. Attempting to download data continuously will result in a
significantly reduced update rate.

Data analysis takes place asynchronously from the data collection so that numerous independent calculations can be carried out very
quickly. An analytical tool kit provides the framework for making navigation decisions. Some of the tools run autonomously, such as
alarm conditions that are evaluated continuously without the need for intervention by the flight controller. Other tools answer high
level navigational questions such as “Is it safe for me to change direction?” or “Which way should I go now?” Tools can be used
sequentially and in combination with status information to form sophisticated strategies that can handle numerous mission
requirements.

## Data streaming

## Alarms

Seven configurable alarms zones can be set within the measuring plane to alert obstacle proximity. Each zone can be set with an
individualized alarm distance, angular width and aiming direction. Typically, one zone would cover 360 degrees around the
vehicle at close range to alert when people get too close to the moving parts. Additionally, a forward looking alarm zone is used
to detect obstacles in the direction of motion. Other alarm zones can check that specific directions are clear of obstructions
before course changes are made.

The status of the alarms can be read from the serial port and any two alarms can be linked to the hardwired outputs. The alarm
zone definitions are stored in non-volatile memory making them available immediately when power is applied. Once the SF40 is
running the alarms are updated continuously without the need for any external commands.

- Alarm zones are created by entering the distance, angular width and direction
- The alarm zones are automatically saved in non-volatile memory and are active immediately after power up
- There are two hardwired alarm outputs and 8 register alarms available through the serial port
- The hardwired alarm outputs can be linked to any of the register alarms
- The alarms register can be accessed from the serial port using the command: ?A

## Virtual laser range finder

The virtual laser range finder (VLRF) tool is used to find the distance in any direction on the measuring plane. VLRF can assist
with keeping station at a fixed distance from a target or measuring how far away an obstacle is. Any number of VLRFs can be
created that aim in different directions. This allows for accurate position holding within a confined space and provides
confirmation of GPS location using adjacent buildings or other known structures as reference points.