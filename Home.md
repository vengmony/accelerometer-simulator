# Introduction #

Accelerometer Simulator is an iPhone/iPod Touch application that transfers accelerometer data from the device to your computer using UDP protocol.

# Use cases #

The main use case for the application is to allow iPhone application developers to create applications that require accelerometer, without having to do all the debugging on the actual device. By inserting two files into their project, they can use the Accelerometer Simulator to provide accelerometer data to their application when debugging on the iPhone simulator.

Second use case is to use your iPhone accelerometer to control something else on another device (such as your MacBook or PC).

Finally you can use the receiver part and write your own data generation application to, for example, execute predefined accelerometer datasets on your application.

# Usage #

  1. Check-out a copy of the source into your XCode development environment
  1. Open the `AccSim.xcodeproj` project file
  1. Make changes to `Info.plist` bundle identifier to match your development certificate. This step is required to install the application on your own iPhone/iPod touch device
  1. Build the application, targeting iPhone device
  1. Run the application on the device.

The application has two views: Accelerometer and Network. The Accelerometer view looks like this

![http://accelerometer-simulator.googlecode.com/svn/wiki/pictures/AccSim1.png](http://accelerometer-simulator.googlecode.com/svn/wiki/pictures/AccSim1.png)

Here you can switch between the actual sensor and manual operation modes. The sliders show the current accelerometer values. In the manual mode you can slide these yourself, generating accelerometer data manually. Note that in the manual mode you can generate data that would not be possible to generate with the HW sensor.

In the second view (tap the bottom tap bar to activate) you can configure network settings.

![http://accelerometer-simulator.googlecode.com/svn/wiki/pictures/AccSim2.png](http://accelerometer-simulator.googlecode.com/svn/wiki/pictures/AccSim2.png)

When the application starts, network is set to **OFF** and no data is being sent. Switching it to **ON** opens the gates and the application begins sending UDP packets to the network. By feault it uses _broadcast_ mode, which means that all devices in the same network (such as your WLAN) will receive the packets. In case you plan to run multiple simulators in the same network, you should switch to _unicast_ mode and specify the target IP address manually. You may also change the port to something else if you wish (remember to change it on the receiving end as well).

Now that AccSim is sending accelerometer data, it's time to setup someone receiving it.

## Embedding into your application ##

To embed Accelerometer Simulator capabilities into your own application, simply add the `AccelerometerSimulation.h` and `AccelerometerSimulation.m` files from the `Simulator classes` directory into your project. Then in the source file where you configure `UIAccelerometer`, simply add
```
#import "AccelerometerSimulation.h"
```
This will override the default behaviour of `UIAccelerometer` when run on the iPhone simulator. When building for device, nothing is changed in your application.

After compiling and running your application on the simulator, it should now be receiving accelerometer data from the AccSim application running on your device.

Should you need to use a different UDP port, you have to edit the `AccelerometerSimulaton.m` file and change the line
```
#define kAccelerometerSimulationPort 10552
```

## Other usages ##

If you want to use the AccSim application for other purposes, check out the `testaccsim.py` [Python-example](http://code.google.com/p/accelerometer-simulator/source/browse/#svn/trunk/Utilities). It prints out the messages sent by AccSim. The format of messages is following:
```
ACC: <deviceid>,<timestamp>,<x>,<y>,<z>
```
The `deviceid` is the unique device id of iPhone, used to identify different sources if there are many (think of a crowd of iPhone users controlling a single game on a big screen...) Next is `timestamp` and acceleration values for `x`,`y` and `z`. These are derived directly from `UIAcceleration`.