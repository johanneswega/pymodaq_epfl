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

- now in the *dashboard* if we move mirror(s), the camera will move!

## Daq Scan 

- Extensions --> do scans --> failed to open it *ImportError: the pytables module is not present*
- it was some version issue with the *venv* --> I created another *env with conda* and python3.11 and it worked (environment name = epfl_intro)

## Second Day - 20.01.26

- today we will learn all about how to write our own *plugin* 
- the *h5browser* is used to look at the data that is recorded using a *scan* --> *hdf5 file* --> binary files in which the data is stored --> saves data inside a *tree* --> contains like a *folder substructure (nodes)*

- we will get the *template* for creating our own *plugin* by *cloning* a *Github repository*, the template repo is located at: *https://github.com/PyMoDAQ/pymodaq_plugins_template* --> click on *use this template* --> *create a new repo* 

- today we will use a special teaching tesmplate repo located at *https://github.com/PyMoDAQ/pymodaq_plugins_teaching* --> *fork* this branch --> untick *copy the main branch only*

- then I cloned the repo using *git clone* 

- now we need to install the *plugins_teaching* into our *pymodaq environment*; however if we just use *pip install* the package will be *installed in its current state* meaning that if we change something in the *.py files* its changes will not take change --> for this we need to install the *plugins_teaching* package using *developer mode*

- *pip* is using the *.toml* file as instructions to install the package --> just a *.txt* file with instructions essentially --> we need to open it and fill in information if necessary, e.g. if it's an instrument or an extention etc. or if additional dependecies and packagaes are required

- to install the package in *developer mode (editor -e mode)* we can run the command `pip install -e .` --> . mean use current path 

- did the installation work? You can use *pip freeze* to list all the installed packages within the evironment

- inside the *scr* directory there are the *source* files we need to modify --> two folders --> *daq_viewer_plugins* and *daq_move_plugins* --> we will have to modfify the templates in there; here we will control a virtual instrument --> spectrometer 

- our virtual instrument --> entrance slit --> colimating mirror --> grating --> focusing miror --> onto exit slit --> at exit slit: photodiode 

- what we want to do --> change angle theta of grating and plot intensity of photodiode --> this will give as the spectrum of the light

- *hardware* folder = place to put the driver for your instrument --> for our example --> *spectrometer.py* --> class that controls communication with the instrument 

- what are modules in python? --> if a folder contains a file *__init__.py* it means you can import it in python --> for instance *hardware* is an importable module containing an file called *spectrometer.py* which contains an *object/class* called *Spectrometer*, so we can import it via `from pymodaq_plugins_teaching.hardware.spectrometer import Spectrometer` 

- we want to now add files from the *pymodaq_plugins_template* so we can *clone* it from GitHub (we do not need to *fork* it since we just want to copy/paste some files form this repo into our teaching repo)s

## Controling the Monochromator

- from the *pymodaq_plugins_template/src/pymodaq_plugins_template/daq_move_plugins/* copy the *daq_move_Template.py* and paste it into your project, i.e. *pymodaq_plugins_teaching/src/pymodaq_plugins_teaching/daq_move_plugins/*

- the we rename the *DAQ_Move_Template* class to something meaningful like *DAQ_Move_Monochromator*

- now we do our first commit: `git add .` and then `git commit -m "your comment"` and then we push it to GitHub via `git push`

- let's actually create a new *branch* `feature/daq_move_instrument` where we will work on the *daq_move* implementations --> `git switch -c feature/daq_move_instrument` to see on which branch you are `git branch`s

- open the *daq_move_Template.py* and go through the TODO's

- after doing TODO's 1-3 Monochromator should be known by Pymodaq --> let's check if we can see it in the list of possible actuators in `daq_move` 

- now let's do a commit

- to inser breakpoints manually `import pdb` and then `pdb.set_trace()` where you want to stop

## Controling the Photodiode 

- the photodiode is a 0D detector, it just gives 1 value, i.e. the intensity
- so again we copy *Daq_Viewer_Template.py* and fill in all the ToDo's
- in the `grab_data()` --> DataToExport --> data --> needs to be list so --> `data = [data_tot]`

## Day 3 - 21.01.26

- everything we discussed yesterday *https://pymodaq.cnrs.fr/en/latest/developer_folder/plugins.html*

- today we will learn how to add *parameters* that control our instruments --> these are input fields in the GUI where we can put our instrument parameters

- depening on the parameter type --> you get different functionalities in the GUI, e.g. if type = list --> then parameter is a drop-down menu with as many entries as the list

- add dictonary for your custom parameter --> then for your 

## Now we will make a Beam Profiler = Pymodaq extensions

