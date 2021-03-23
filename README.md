# Citadel
Home automation system, providing comfort and security features

Citadel is an integrated system that aims to control various end devices such as automatic doors, door locks, window blinds, heating valves etc. based on different inputs like wind speed sensors, temperature sensors, air quality sensors etc in conjunction with the conditions set by the user, all while providing convenient and seamless user interface.

## Interface
The interface of Citadel is web based which makes it useable on almost any device connected to the home network. 

## Core
Citadel ___Core___ is the module that handles all logic and calculations, decisions and interface requests. It also provides API for third party modules. MQTT is used for communication between ___Core___ and modules. Initial consideration for communication between the interface and ___Core___ is to use Potoo (https://github.com/DexterLB/potoo).

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
* Door vicinity failsafe sensor
* Optional door lock
* Optional wall touch panel
* Optional wall RFID reader
* Optional door lighting

Current hardware considerations are for sliding door. However a version for automated traditional doors will be added as well.

### Module _Weather_
___Weather___ is another 

### Module _Lightswitch_
___Lightswitch___ is another simple module. Each instance controls a lighting segment according to ___Core___ provided ruleset or manual override via the web interface or the integrated panel.

Hardware consists of:
* Programmable controller
* 

### Module _Air quality_

### Module _Alarm_

### Module _Curtains_

### Module _Air conditioning_

### Module _Kingsley_

This module interprets voice commands impersonating a virtual butler


## Hardware modules

### Programmable controller
Citadel system programmable controllers are the nerves that translate the commands provided by ___Core___ to the actual devices, such as actuators and light relays, fetch the feedback data from the feedback sensors (i.e. door blocking failsafe sensors) and the data from environment monitor modules such as the ___Weather Station___ module. The programmable controllers are basically a minified version of Arduino, with only the relevant functions kept and integrated network connection (as opposed to a separate network shield). The controllers, albeit basically the same for the different peripheral modules, are an integral part of the respective periferal's PCB, in order to minimize the form factor and simplify usage and deployment.


