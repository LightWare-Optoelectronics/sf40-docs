# Functional description

## Overview

The SF40/C uses a scanning laser rangefinder to measure on a 360 degree disc with a radius of 100 meters. Collected data is stored in memory and continually refreshed as the laser scans around. The speed of rotation is 5.5 revolutions per second at a resolution of +- 3 cm. This data can be used to trigger predefined alarm zones, give information about target distances in any direction or streamed to a host controller with selectable output rates.

## Data streaming

Measurement data accumulated by the SF40/C can be output to a host controller for immediate or deferred analysis. The rate at which data is output can be selected as 20010, 10005, 6670 or 2001 points per second.

## Alarms

Seven configurable alarms zones can be set within the measuring plane to alert obstacle proximity. Each zone can be set with an individualized alarm distance, angular width and aiming direction. Typically, one zone would cover 360 degrees around the vehicle at close range to alert when people get too close to the moving parts. Additionally, a forward looking alarm zone is used to detect obstacles in the direction of motion. Other alarm zones can check that specific directions are clear of obstructions before course changes are made.

The status of the alarms can be read from the serial port through a command or from the streaming data. Once the SF40/C is running the alarms are updated continuously without the need for any external commands.

## Virtual laser range finder

The virtual laser range finder (VLRF) tool is used to find the distance in any direction on the measuring plane. VLRF can assist with keeping station at a fixed distance from a target or measuring how far away an obstacle is. Any number of VLRFs can be created that aim in different directions. This allows for accurate position holding within a confined space and provides confirmation of GPS location using adjacent buildings or other known structures as reference points.