# How to Structure a Custom Swerve Drivetrain
>[!NOTE]
> This isn't required reading, so you can move on if you don't care about this, but I do recommend that you come back to this at some point and read it

The structure of a custom swerve drivetrain is deceivingly complex, and the process is pretty simple using wpilib's integrated kinematics classes.

>[!NOTE]
> for more info about the kinematics involved, read [this post on Chief Delphi](https://www.chiefdelphi.com/t/swerve-drive-direct-and-reverse-kinematics/395803/3)
-  We need 2 abstractions on top of running motors to make this work. First, we should have a class to hold the state (the velocity and angle) for each module, and set the motors to that state. 
- Then, we should have a class that can take the desired velocities for the whole *drivetrain* and set each module to the correct state to get there 
## `SwerveModule` and `SwerveModuleState`
1. First, we create a class to handle the state of the swerve module, ie. it's velocity and angle. 
	1. It'll just store a pair of doubles for the velocity and angle
2. Then, we need to create the class that handles using the state to set motors
	1. so we create the `SwerveModule` class, that takes two motors for drive and turn
	2. and we create a method to set the contents of a `SwerveModuleState` to the motors
		1. just set the drive motor to the requested speed and the steer motor to the requested angle
## The full Swerve Drivetrain
Now it's time to bring all the swerve modules together to drive. But first, some theory.

Swerve Drivetrains can be in 3 different situations. Either they're translating, which puts the modules in this position:

<img src="images\SwerveTranslating.png" alt="drawing" width="200"/>

Spinning, which puts them in this position:

<img src="images\SwerveSpinning.png" alt="drawing" width="200"/>

Or they're Spinning while Translating, which puts them in this position: 

<img src="images\SwerveSwerving.png" alt="drawing" width="200"/>


We need a system to be able to handle each of these cases, and figure out what states each module should be in. This involves some [vector math](https://www.chiefdelphi.com/t/swerve-drive-direct-and-reverse-kinematics/395803/3) that we fortunately don't have to think about, since wpilib gives us the `SwerveDriveKinematics` class.

- Much like `DifferentialDrive.arcadeDriveIK()` from the minibot project, `SwerveDriveKinematics` turns the inputs from the joystick (x, y and rotation velocities) into inputs that we can actually use to set the motors (Swerve Module States) 
- for more info on how exactly to use it, you can consult the [wpilib docs](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/index.html) (and the respective subtabs for swerve stuff)


The `SwerveDriveKinematics` class takes data about the positions of each of your modules in order to do this calculation, along with the actual x y and rotation velocities that you desire

We can then feed the output from `SwerveDriveKinematics` into each of our module classes, where they will set the motors directly.

Finally, we have a drive function that will drive the swerve drive!
