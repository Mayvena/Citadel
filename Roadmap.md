# Roadmap

This file outlines the tasks that need to be done and will attempt to keep track of their progress.

1. Proof of concept prototypes of hardware devices. This step is necessary in order to have test ecosystem in which the software shall be developed. 
    1. _Light switch_ - solid state relay module, small touchscreen panel, network shield, all controlled by Arduino. The aim is to have touch buttons in the touch screen that would change the state of the relay module, as well as providing information about its state and interface for network control
    2. _Lock_ - solenoid lock module, small touchscreen panel, RFID shield, network shield, all controlled by Arduino. The aim is to have touch buttons and operation modes that provide control options for the lock - RFID, button touch, code input - as set in a settings menu; also controlleable via network interface. RFID identification can be local or centralized, using network requests for identification
    3. ...
2. _Core_ module prototype - basic structure for communication with the hardware devices
3. _Kingsley_ module prototype
4. Once there is some clarity on the above modules, a decision regarding the environment must be made. Initial considerations are for Ubuntu Server, running MySQL database and Python or NodeJS (or both) for development of the actual system
