# vivint-2gig-home-sec
Repository for re-using vivint equipment
# Re-using vivint equipment
## Introduction

Vivint provides decent hardware that is more often than not locked into their proprietary ecosystem. Here you will find information that outlines how to use vivint equipment with different services

## Doorbell Camera


## Vivotek Cameras

### General

Many of the older Vivint cameras are rebranded Vivotek devices, including Vivint's doorbell camera.

### List of Devices

* Vivint Doorbell Camera (DBC2-43536D)
* Vivint Outdoor Camera v1 (HD300W) - Discontinued
* Vivint Outdoor Camera v2 (HD400W or IB8363-W) - Discontinued
* Vivotek IP Camera (ADC-V520IR or IP8131W) - Discontinued
*

The Vivotek cameras can be connected to your wifi via WPS or by using the camera's AP mode.

The cameras run busybox, which is a stripped down implementation of linux. Many of utilities do not provide full functionality in order to create a low memory and fast operating system for embedded devices. Many of the commands are actually compiled into a single binary that uses aliases in order to invoke different functionality.

The cameras use a simple `getparam` and `setparam` API that changes config files in /etc/. If you enable telnet, you modify the files directly with `vi`

#### Resetting your camera

Reset the camera by holding down the WPS button for 30 seconds. The status LED should blink red/green before releasing the button.

#### Enabling Access Point Mode

To enable access point mode on your camera, simple hold down the available button on the camera for 30 seconds to reset the camera. When the camera reboots, it will be in access point mode and will provide a WiFi network without encryption

#### Enabling Telnet

Enabling telnet can be done by simply using the camera's API by accessing the following URL with your browser or HTTP request tool of your choice

    http://root:adcvideo@{ip_of_camera}/cgi-bin/admin/mod_inetd.cgi?telnet=on

If you are prompted for a username and password, use `root` and `adcvideo` respectively

Once telnet is enable, you can connect directly to the cameras by using the command

  telnet {ip_of_camera}

  Most of the configuration can be found in the /etc/conf.d/ folder in XML

### Doorbell Camera (DBC2-43536D)

![](https://www.vivint.com/sites/default/files/styles/small_hq/public/image/2020-03/Scareawayporchpiratespov.webp?itok=SkzEJVTL)

LED Status:

TODO Insert table of LED status for the doorbell camera

#### Doorbell Chime

When you have a vivint doorbell and disconnect the vivint head-unit, you will no longer hear it "chime" when somebody presses the doorbell. This was a problem for me so I started exploring how to intercept the chime "event" at the doorbell in order to make a speaker inside "chime".

The doorbell runs linux with busybox. Busybox is a low resource, stripped-down implementation and its commands do not seem to support all the options.

1. When the doorbell is pressed, the script `play_sound` is invoked by `chronos`, with the argument `/etc/audio/bell.wav` (in my case).

2. The `play_sound` script is stored on a read-only portion of its storage. 

3. The `play_sound` script invokes `nice`. I managed to `override` `nice`. You can detect when `nice` is invoked for playing `/etc/audio/bell.wav` and when it is invoked for playing some other `audio` file. Using `ssh`, you could for example connect to a Raspbery Pi (with speaker attached) when `nice` was invoked for playing `/etc/audio/bell.wav`.

### Vivint Outdoor Camera (HD300W or HD400W) - Discontinued

Outdoor Camera v1

Outdoor Camera v2

![](https://github.com/ilektron/vivint-2gig-home-sec/blob/master/images/outdoor_camera_v2.jpg?raw=true)

These cameras are no longer installed at new locations and have been replaced by the new Vivint Outdoor Camera, which is probably the Ping camera that has been repackaged

LED Status:
- Yellow Solid -> System Starting
- Green Solid -> Connected to Vivint panel
- Red Flashing -> Unable to connect to Vivint Panel
- Flashing Yellow -> WPS enabled
- Green/Blue Flashing -> AP mode enabled
- Red/Green Flashing -> Processing button press or resetting camera

### Vivint IP Camera (ADC-V520IR or IP8136W) - Discontinued

#### Specifications

* 1-Megapixel CMOS Sensor
* Compact Design
* Real-time H.264 and MJPEG Compression (Dual Codec)
* 30 fps @ 1280x800
* Two-way Audio
* Removable IR-cut Filter for Day and Night Function
* Built-in IR Illuminators, Effective up to 6 Meters
* Built-in MicroSD/SDHC Card Slot for On-board Storage
* Built-in IEEE 802.11b/g/n WLAN
* Wi-Fi Protected Setup (WPS) for Easy and Secure Wireless Network Connection

## Ping Family of Cameras

The newer Vivint cameras use NIPCA, which stands for [Network IP Camera Application.](http://gurau-audibert.hd.free.fr/josdblog/wp-content/uploads/2013/09/CGI_2121.pdf). Some D-Link WiFi cameras also use NIPCA. NIPCA seems to be closed source. The Ping cameras do use some end points that don't seem to be included in the NIPCA standard. Any endpoints discovered that aren't listed in the NIPCA documentation will be documented below

### Vivint Ping

#### Enabling telnet

Simply make an HTTP request to the following URL. It should return a 200 code as well as indicate that telnet is on. Ping cameras use digest auth for their API with a default username/password of `admin:admin`

```
http://(ipaddr)/cgi/admin/telnetd.cgi?command=on
```

Next, you can telnet to the device

    telnet root:admin

#### Enabling Web Interface

With root access, you can now enable the web interface to configure the camera.

1. Enable the web interface in the database via `tdb`

```
# tdb set HTTPServer WebAccess2_byte="1"
```

2. Restart the web server

```
# /etc/rc.d/init.d/lighttpd.sh restart
```

You should now be able to access the web interface via the IP of the camera, which should prompt you for the username and password, admin:admin



