# Citadel
Home automation system, providing comfort and security features

Citadel is an integrated system that aims to control various end devices such as automatic doors, door locks, window blinds, heating valves etc. based on different inputs like wind speed sensors, temperature sensors, air quality sensors etc in conjunction with the conditions set by the user, all while providing convenient and seamless user interface.

## Interface
The interface of Citadel is web based which makes it useable on almost any device connected to the home network. 
_Most likely the interface will be Node.js based_

## Core
Citadel ___Core___ is the module that handles all logic and calculations, decisions and interface requests. It also provides API for third party modules. MQTT is used for communication between ___Core___ and modules. _Initial consideration for communication between the interface and_ ___Core___ _is to use Potoo (https://github.com/DexterLB/potoo)._

Further reading: https://mqtt.org/

## Software Modules
Each module controls one type of end devices and can be instantiated once for each particular device (as many of the supported device types can be represented by more than one in the local setup - like doors or blinds.

### Module _Lock_
An instance of this module controls every lock of a simple (non-automatic) door. Basically it's a simplified version of the ___Door___ module.
_Note:_ This module might actually get scrapped and ___Door___ might be implemented in a way that allows it to just operate locks

### Module _Door_
Every automatic door is controlled by an instance of this module. Possible modes are:
* Auto open and auto close
* Auto open, manual close
* Manual open, manual close
* Manual open, auto close
* Passcode open, auto close
* Passcode open, manual close
* Keycard open, auto close
* Keycard open, manual close
Auto open can be adjusted to allow variations for the size of the person (e.g. allowing humans to pass, but not pets, etc.).
Auto close times can be adjusted

Hardware consists of:
* Door actuator
* Door proximity sensor
* Door block failsafe sensor
* Optional door lock
* Optional wall touch panel
* Optional wall RFID reader
* Optional door lighting

Current hardware considerations are for sliding door. However a version for automated traditional doors will be added as well.

### Module _Weather_
___Weather___ is an information module that controls the data stream from a ___Weather Station___ hardware module. It provides ___Core___ with parameters such as outside temperature, humidity, light, wind speed, etc. 

### Module _Lightswitch_
___Lightswitch___ is another simple module. Each instance controls a lighting segment according to ___Core___ provided ruleset or manual override via the web interface or the integrated panel.

Hardware consists of:
* Programmable controller
* Relay module
* Touchscreen panel

### Module _Air quality_
This module will monitor air quality in each individual room via CO, CO2, FDP and humidity sensors and control the ventilation system mechanisms to maintain optimal levels.

### Module _Alarm_

### Module _Curtains_
Each instance of this module controls the curtain actuators of a window. There has to be a way to issue commands en masse to a few instances at once, grouped by some parameter (e.g. _"East windows"_).

### Module _Shutters_
This module is similar to ___Curtains___, but since it controls the exterior shutters and blinds, it is used in a more security-related aspects. Used to shut the house down when everyone has left and the ___Alarm___ module is on, used by ___Alarm___ module to simulate presence by opening and closing in certain pre-programmed patterns. Used by ___Heating___ module to preserve energy (in the role of more efficient curtains) and by ___Weather___ module to provide protection of the home from hails and storms.
Hardware is similar to that of ___Curtains___, however there's an added option of locking the shutters in closed position with a solenoid lock to prevent forcing them open. Actuators are a heavier duty ones as well, as to accomodate the option for heavier intrusion-proof shutters.

### Module _Heating_
Module ___Heating___ controls the heating of the house. Initial version will be built around controlling thermal pump central heating, but other modes will be added as well. Because of the large number of various heating methods that will need to be covered, most likely this module will feature a set of submodules, each for a specific type of heating. 

### Module _Kingsley_

This module interprets voice commands and provides voice responses impersonating a virtual butler. 
_Pending steps:_
* Find an open source voice recognition software. Preferrably one that can work offline.
* Check for ways to implement different languages. 
* Would be nice to have it automatically recognize the language being used by user.

Some reading material: https://www.geeksforgeeks.org/voice-assistant-using-python/


### Module _Audio_

This module controls a set of audio speakers scattered around the house, providing audio interface to other modules and routines. Takes as parameters the ID of the speaker and an audio stream that is to be played through it. Can be used by ___Kingsley___ to provide voice replies to queries, to stream music, radio, messages etc. 
Also should have the option to connect to YoutubeMusic and Spotify APIs to play user's playlists.

### Module _Tracking_

This module aims to provide location data for each user in the premises. Most likely by using a combination of visible light cameras and IR motion detectors. It should be able to recognise a registered user and track their motion throughout the house, as well as pets' movements and location. The goal is to provide location per request of other modules, such as ___Audio___ and ___Kingsley___ for audio feedback, music and lighting that follow the user while moving around, etc. functionalities.

Additional consideration: think of a way to detect the location of mobile devices, connected to the network - this will allow to contextualize the mobile version of the interface. E.g. use a common lightswitch button in the interface that controls the local light segment.

### Module _Mousedroid_

This module controls mobile units around the home - like lawnmowers, vacuum cleaners, moping units, etc. Uses the Rover variant of Ardupilot open source software and a combination of GPS navigation and radiobeacon triangulation, as well as environment mapping algorithm and user-set coordinates of the area of activity. 

### Module _Area_ 

Interface module, used to set up zones for the activities of the various mobile uints, e.g. the garden for the lawnmower etc.


A similar open source project to check out: https://www.openhab.org/


## Hardware modules

### Programmable controller
Citadel system programmable controllers are the nerves that translate the commands provided by ___Core___ to the actual devices, such as actuators and light relays, fetch the feedback data from the feedback sensors (i.e. door blocking failsafe sensors) and the data from environment monitor modules such as the ___Weather Station___ module. The programmable controllers are basically a minified version of Arduino, with only the relevant functions kept and integrated network connection (perhaps using a ENC28J60 LAN Ethernet Network Board Module - probably a modified version with a more secure way to plug the UTP cable instead of an RJ45). The controllers, albeit basically the same for the different peripheral modules, are an integral part of the respective periferal's PCB, in order to minimize the form factor and simplify usage and deployment.

### Control panel
The universal control panel is a fixed mini computer, most likely based on a Banana Pi Zero WH or a Raspberry Pi Zero with a 7" or a 10" touchscreen panel. It will run a minified Raspbian distribution with a Cog browser displaying the ___Interface___ module. Considering two variants:
* Fixed - this one is fixed to a wall and connected to the network via ethernet cable and powered through power wiring
* Mobile - featuring an integrated LiPo battery, balancer, and a USB charging port. Also considering a wireless option for charging, using a dedicated stand. It will use WiFi to connect to the network and fetch the interface. 
Still not sure whether a mobile version is actually needed, as the goal is to allow the interface to be accessible from any mobile device in the local network, such as smartphones and tablets.

### Weather Station
This module consists of a few separate hardware devices in a common package that provide a realtime stream of meteorological data. The devices measure:
* Air temperature
* Sunlight
* Sun direction
* Air humidity
* Wind speed
* Wind direction

### Lawnmower
An Ardupilot-based (Rover variant) control system, interfacing with a lawnmower drive motor and controls; using a combination of the Ardupilot satellite navigation and an additional beacon triangulation module for added precision. Features graphical interface for setting up zones of operation on a satellite map.

### Lawnmower dock
A docking station for the ___Lawnmower___, featuring a power recharge system for the ___Lawnmower___ and a radio beacon that allows it to triangulate its position in relation to the dock. Used both as a means of more precise positioning in the serviced area and as a way to locate the dock itself when recharging is needed.

### Audio player
This is an Arduino-based IP audio player. It consists of a ___Programmable controller___ module, a DAC, an audio amplifier (perhaps TPA3144D2 based) and a speaker (most likely an IP67 specific one for ceiling montage).
Some food for thought: 
* https://circuitdigest.com/microcontroller-projects/arduino-audio-music-player
* https://www.schematics.com/project/6w-inductor-free-stereo-btl-class-d-audio-amplifier-circuit-diagram-27741/

## Supply links:

### Network interface
* https://www.aliexpress.com/item/32548974618.html
* https://www.aliexpress.com/item/32759613417.html
* 
