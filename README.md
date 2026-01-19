# Notes and Code for Pymodaq Introduction Course at the EPFL

This is some note. I am running the pymodaq on Debian. 

I used the *mamba* and *mini-forge* environment as explained on the website. However, I had to install pyqt5 system-wide with `apt-get install` to be able to open e.g. the *daq_viewer*. Also I have to run `sudo daq_viewer`for it to work.

In the end I just used `venv` to make my own environment running on python 3.9

## Connecting a new instrument - Avantes Demo

- need to install device driver (if needed)
- on linux you get the .so file 
- on Windows you might need to install the software of the device
- if someone has already written a plugin for the device you can install it using `pip install pymodaq-plugins name_of_plugin`

## Change how windows look like 

- open daq move --> zahnrad --> actuators --> Ui : Original 
- now the window looks bigger like the original pymodaq 
- like this you can configure any presets --> sometimes with the horizontal bar icons

## We will build our own Dashboard

- we need the device plug-in's! How do we know which plug-in's are installed --> either use *plugin_manager*  

- for most instruments you need *dll* or *.so* drivers --> manifacturer will tell you how to do it. Note: USB serial devices normally do not require a driver

- you need the *sdk* (software developing kit)

- you can use either the *plugin_manager* or you go to the pymodaq website and then *supported instruments* then you can go to the plugin you want on the GitHub page and then install it with the *pip* command that is written there

- we will install the *pip install pymodaq_plugins_mockexamples*

- then open *daq_viewer* --> *gear* --> *DAQ_type : DAQ2D* --> *Detector: BS Camera* --> *init. Detector* --> *play*

- zooming : either mouse wheel or hold right click and zoom

- ROI : region of interest --> can add rectangles/circles/ellipses and stuff inside them is analyzeds

- now let's go to the *dashboard* and create our own *preset* --> *preset modes* --> *new preset*

- give your preset a reasonable name; we will have our mock experiment consisting of a mock camera and a mock piezo mirror. *Detectors: Plugins --> Mock --> 2D --> BS Camera*, *Acctuators: Plugins --> Mock -->  Piezo Mirror* (add two one for X and one for Y)

- if multiple instruments/acctuators/detectors use the same serial port/communication, you need to set one of them as *Master* and the others as *Slaves* --> when editing the preset --> set e.g. the X mirror as *Master* (under *status* just above X or Y) --> note the random number *device ID* and put the other instruments as *Slave* with the same *device ID*s (also the camera)

- now in the *dashboard* if we move mirror(s), the camera will move!s