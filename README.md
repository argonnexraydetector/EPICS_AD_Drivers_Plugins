
# EPICS AD Drivers & Plugins

A driver from APS Detector Group 

## PCO Driver
The PCO driver was commissioned by Imaging Group at APS/XSD (i.e., 2-BM, https://www1.aps.anl.gov/imaging) for the PCO Edge and Dimax camera which are used for X-ray Micro-tomography. It has been extensively been tested at 2-BM by Xianghui Xiao and Tim Madden. This driver has been developed using the low-level Camera Link libraries. It currently runs on Windows, but we are working to port the driver to linux. (Note: It was developed before PCO released their Windows-based SDK.) We are currently working to merge this driver into the official EPICS AreaDetector repo. 

The PCO Driver has three parts:
1. Camera Link driver for Area Detector (cameraLinkSrc2)
2. Camera Link serial port (camLinkSerialSrc)
3. PCO camera source (PCO2src)

### Camera Link Driver 
Camera Link has two parts: a serial port for camera control, and a high speed data link for image data. The Camera Link driver is a class ADCameraLink, which inherits ADDriver, and adds functions to interface to any camera link card. There are special class for the silicon software MEIV grabber and Dalsa/Coreco grabbers. Editing the makefile sets which grabber is being used. 

For supporting a specific camera, one writes a driver that inherits ADCameraLink and adds the serial port code to control the camera.

The Camera Link Driver is located in: cameraLinkSrc2

### CameraLink Serial Port
The serial port on the camera link board shows up in EPICS as an asyn serial port driver. The port of this driver is passed to the the camera driver. The serial port driver behaves like other asyn serial port drivers as documented in
http://www.aps.anl.gov/epics/modules/soft/asyn/R3-1/asynDriver.html#drvAsynSerialPort

The camera link serial code is located in: camLinkSerialSrc

### PCO Camera Driver
The PCO camera driver in PCO2Src has all the serial port control code for the PCO Dimax and PCO Edge cameras. To build:
1. Make in cameraLinkSrc
2. Make in camLinkSerialSrc
3. Make in pluginSrc
4. Make in PCO2src

## Plugins
### Edge Camera Plugin
The PCO Edge camera sends scrambled images that need to be descrambled. This plugin handles this. If the camera runs faster than the plugin, then images will be stored in the NDArray queues.

