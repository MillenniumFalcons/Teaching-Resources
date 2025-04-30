# How To Run Falcons and Krakens
Falcons and Krakens are controlled by TalonFX motor controllers. Therefore you must use the `TalonFX` class to run these motors

## Before you Begin:
1. you must install the vendor library for TalonFX, which is called Phoenix 6. 
	1. the library might be available in the vendor dependencies tab in wpilib, which looks like this <img src="images\VendorDepTab.png" alt="drawing" width="20"/>
	2. If it isn't you'll have to look up the [website](https://v6.docs.ctr-electronics.com/en/stable/docs/installation/index.html) and follow the instructions for online installation
## Running TalonFXs
1. create a `TalonFX` object
	1. the first parameter is the can ID of the controller, which can be found using [Phoenix Tuner](https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/index.html) and connecting to the robot wifi
		- you may have to set the team's roborio IP adress (10.36.47.2) in the "Enter a team # or IP" section in the hamburger menu
		- you may also need to run the temporary diagnostic server if there isn't any robot code deployed, which can be accessed under settings -> frc advanced -> run temporary diagnostic server
		- more info can be found [here](https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/connecting.html) 
	2. the second parameter (which is optional) is the can bus name. 
		- usually, we put swerve drive motors on a canbus called "drive", and subsystem motors on another canbus called "subsystems"
2. the main method of running TalonFX motors will be thorugh [*Control Requests*](https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/api-usage/control-requests.html) 
	- the docs here are excellent, and **you should read them before continuing.** The rest of the section will assume you already know generally what a control reqeust is and how to use it
3. To use the control request in our subsystems:
	1. fist have one generic `ControlRequest` object per motor in periodicIO (this fullfills the same role as the leftOutput and rightOutput in the minibot drivetrain subsystem)
	2. next create specific control requests in PeridoicIO to fullfill the specific types of output you want to set
		- for example, a duty cycle output would use `DutyCycleOut`
		- be sure to create one specific control request per motor
	3. in the setOpenLoop function, instead of using `periodicIO.leftOutput = leftOutput`;, we will now say that `periodicIO.leftGenericControlRequest = periodicIO.leftDutyCycleRequest.withOutput(leftOuptut);`
		- similar to setting the periodicIO double in the NEO version, except instead of just setting the output double, we also specify what the control type is based on the type of control request we use
		- *if you don't understand any of this, ask for help or [consult the docs](https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/api-usage/control-requests.htm) or both*
4. **be sure to call `setControl()` in `writePeriodicOuputs()` or in some other periodic method, so it runs repeatedly.**
> [!NOTE]
> This is the method for **TalonFx Motors ONLY** and does not work for motors controlled by Spark motor controllers (eg. NEOs, NEO Vortexs, NEO 550s, etc.)

> [!NOTE]
> to run the same position PID you do for step 3 of the training, you must change the type of the control request from `DutyCycleOut` to `Position` and set the generic control request equal to the new control Request
## Configuring TalonFX Motors
this process is very similar to the way configuring NEOs works
1. TalonFXs are configured using a `TalonFXConfiguration` object
2. this object is loaded with all the configuration details, then passed off to the TalonFX itself using the `<name of Motor>.getConfigurator().apply(TalonFXConfiguration config)` method
#### The `TalonFXConfiguration` object
This object contains many methods that can set various details of the motor. For now, the important ones are:
1. the invert
	- usually, applying a positive voltage to the motor makes it turn counterclockwise. But if we want a positive voltage to result in "positive" motion (eg. forward on a drivetrain or upward on an elevator) we may need to change that
	- use `<Name of Config object>.MotorOutput.Inverted = InvertedValue.whatever` to invert the motor from counterclockwise to clockwise for positive voltage.
		- instead of using a Boolean like Spark maxes, the Talons use an Enum called InvertedValue so u can specify if positive voltage turns the motor counterclockwise (`InvertedValue.CounterClockwise_Positive`) or clockwise (`InvertedValue.Clockwise_Positive`)
2. Neutral Mode
	- this controls what happens when the motor isn't running any other command. 
	- it can be set to coast, which doesn't restrict any outside movement or brake, which does
	- It is done with `<Name of Config object>.MotorOutput.NeutralMode = NeutralModeValue.kBrake or kCoast`
3. PID gains
	- this will make more sense after you've completed step 3, so bear that in mind
	- the Talons can run a PID controller onboard at a much faster rate than the roboRio can, and we can configure the PID gains for it here
	- use `<name of Config object>.slot0.withKP(double), .withKI(double)` and `.withKD(double)` to set them
>[!TIP]
>you can easily tune PID gains for the Talons using phoenix tuner by clicking on the motor and going to the output tab

>[!TIP]
>Talons also support having multiple "slots" of PID, which allows you to set different sets of gains, and switch between them through a variable in every control request.

