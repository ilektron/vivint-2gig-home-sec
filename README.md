# vivint-2gig-home-sec

Repository for re-using Vivint and 2GIG equipment

## History Lesson

Vivint started as APX alarm, and began selling resedential alarm systems directly to consumers. Eventually, as they expanded, they rebranded from APX to Vivint.

2GIG stands for 2 guys in a garage. They saw deficiencies in Honeywell devices and a market opportunity, so they started designing their own hardware in one of their garages. After they grew, Vivint acquired the 2GIG Go!Control panel successor technology. 2GIG then created the Go!Control3 as a successor to the Vivint Skypanel that had been acquired. 

# Vivint Specific Equipment

## Introduction

Vivint provides decent hardware that is more often than not locked into their proprietary ecosystem. Here you will find information that outlines how to use Vivint equipment with different services

## Cameras

For more information about the cameras that Vivint uses, go to [./devices/vivint/cameras/README.md](the cameras README)


## Sensors

The older Honeywell based sensors can be listened to via a Software Defined Radio (SDR). A popular project, rtl_433, can listen to sensor packets and forward them to an MQTT server, which also can be read by Home Assistant or other smart home platforms.

The sensors use a 345MHz center frequency, Manchester encoded OOK transmission. 

rtl_433 also has a script that can be run to generate devices in Home Assistant


### More specific information
[Vivint Sensors](./devices/vivint/sensors/README.md)
[2GIG Sensors](./devices/2gig/sensors/README.md)


# 2GIG Equipment
