NEO motors are controlled by SparkMax motor controllers. Therefore you must use the `SparkMax` class to run these motors

## Before you Begin:
1. you must install the vendor library for sparkmaxes, which is called RevLib. 
	1. the library might be available in the vendor dependencies tab in wpilib, which looks like this <img src="images\VendorDepTab.png" alt="drawing" width="50"/>
	2. If it isn't you'll have to look up the [website](https://docs.revrobotics.com/brushless/revlib/revlib-overview) and follow the instructions for online installation
> [!NOTE]  
> The Revlib website might be blocked on school wifi, so you may have to use a vpn or hotspot or some other method to get around it.
## Running sparkmaxes
1. create a `SparkMax` object
	1. the first parameter is the can ID of the controller, which can be found using [Rev Hardware Client](https://docs.revrobotics.com/rev-hardware-client) and plugging a usb-c cable into the port on one of the spark maxes
	2. the second parameter should be `MotorType.kBrushless` for all NEO motors
2. the main method of running spark max motors will be to use the `setReference()` method
	1. to access the `setReference()` method, you have to get the closed loop controller from the `SparkMax` object using `<Name of SparkMax>.getClosedLoopController()`
	2. you can then call the `setReference()` method to set the motor output
3. the `setReference()` method has 2 parameters, a double for the output and a `ControlType` for the mode that you want the motor to run in
	1. there are many different modes for controlling voltage, velocity, position, and current, among others
	2. for now, you'll be using `ControlType.DutyCycle`, which  corresponds to percentage of the motor's peak voltage
4. **be sure to call `setReference()` in `writePeriodicOuputs()` or in some other periodic method, so it runs repeatedly.**
5. you can use any method to set the values used in `setReference()`, but in general, we use a double for each motor in a subsystem, and then modify them using functions that are available
	- for more info about this, consult [**the doc**](https://docs.google.com/document/d/1V-01oLd14zoN3VbUrFwmir2e-0ZzLylEoiRZNfv4-KQ/edit?usp=sharing) 
> [!NOTE]
> This is the method for **Spark Maxes ONLY** and does not work for motors controlled by TalonFX (eg. Falcons, Krakens, Mini Krakens, etc.)

> [!NOTE]
> to run the same position PID you do for step 3 of the training, you must change the `ControlType`, (the second parameter in `setReference()` ) to `ControlType.kPosition` 
## Configuring SparkMaxes 
1. SparkMaxes are configured using a `SparkMaxConfig` object
2. this object is loaded with all the configuration details, then passed off to the SparkMax itself using the `<name of SparkMax>.configure()` method
#### The `SparkMaxConfig` object
This object contains many methods that can set various details of the motor. For now, the important ones are:
1. the invert
	- usually, applying a positive voltage to the motor makes it turn counterclockwise. But if we want a positive voltage to result in "positive" motion (eg. forward on a drivetrain or upward on an elevator) we may need to change that
	- use `<Name of Config object>.inverted(boolean)` to invert the motor from counterclockwise to clockwise for positive voltage.
2. Neutral Mode
	- this controls what happens when the motor isn't running any other command. 
	- it can be set to coast, which doesn't restrict any outside movement or brake, which does
3. PID gains
	- this will make more sense after you've completed step 3, so bear that in mind
	- the SparkMaxes can run a PID controller onboard at a much faster rate than the roboRio can, and we can configure the PID gains for it here
	- use `<name of Config object>.closedLoop.p(double)`, `<name of Config object>.closedLoop.i(double)` , and `<name of Config object>.closedLoop.d(double)` to set them
