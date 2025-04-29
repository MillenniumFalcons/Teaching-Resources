# Step 2: minibot drive

1. Create a new file called Drivetrain.java and write a Drivetrain class that uses the interface [PeriodicSubsystem](https://github.com/MillenniumFalcons/2023-Minibot/blob/master/src/main/java/team3647/lib/PeriodicSubsystem.java).   
2. In your drivetrain class, make a class called periodicIO.   
   3. This will be used to store variables related to the inputs or outputs, such as the variables for how fast the left and right motors should run.   
3. 4. tialize the left and right motors (there’s 2 motors and each motor controls 2 wheels on either side)   
   5. **If leads aren’t reachable, use [this guide](Supplementals/How%20to%20run%20NEO%20motors.md) to see how to run NEO motors**  
4. 6. etting Motors:** use the setReference method to run the motors in writePeriodicOutputs.   
   7. writePeriodicOutputs() is a method included in PeriodicSubystem for you to override. It runs periodically (once per robot loop)   
   8. Make sure the setReference method is running repeatedly (eg. in writePeriodicOutputs() ) and the only thing you change is the output that it’s running at.   
5. 9. rive Method:** The drive method should take doubles for speed and rotation (turning).  
   10. Create a variable called wheelspeeds and use the DifferentialDrive.arcadeDriveIK() method and plug in your values. The arcadeDriveIK method returns Wheelspeeds, which is a class that represents the speed of each wheel.   
   11. Use wheelspeeds.left and wheelspeeds.right to reference the variables for left and right speeds  
   12. set the values for left and right speeds in periodicIO to those. 

**Commands:** 

6. Create a new file in the commands folder called DrivetrainCommands.java and create a drive command which uses the drive method in the drivetrain class.  
7. 1. Your class should take a Drivetrain object as a parameter  
8. 2. **If you don’t know how to make a command, follow [this guide](Supplementals/How-to-Make-a-Command.md) to learn how. [Make sure you know what a command is conceptually](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html) before continuing**  
9. Your command should take in [DoubleSuppliers (see this for more info on what a doublesupplier is)](https://docs.wpilib.org/en/stable/docs/software/basic-programming/functions-as-data.html) for drive and rotation speeds, and call your drivetrain subsystem’s drive method, passing those parameters in using \<Name of DoubleSupplier\>.getAsDouble()

**Bindings**

8. This should take place in your RobotContainer file.   
9. 1. **If you don’t have a RobotContainer file, you have chosen the wrong type when creating a project. You now must create a new project and copy your code to that.**   
10. 2. [**Make sure that you follow the directions from here**](Supplementals/Intro-to-Wpilib.md)

**In RobotContainer:**

9. create a drivetrain object and a DrivetrainCommands object.   
10. register the subsystem (drivetrain) using CommandScheduler.getInstance().registerSubsystem();   
11. make a default command for your drivetrain in the constructor using \<name of drivetrain object\>.setDefaultCommand(\<name of drive command\>)  
12. Make a Joysticks object ([copy the class Joysticks from one of the robot code Githubs](https://github.com/MillenniumFalcons/2023-Minibot/blob/master/src/main/java/team3647/lib/inputs/Joysticks.java))  
    1. The joysticks object is how you get the input from an xbox controller  
    2. To create a new joysticks object, use the constructor and pass in port 0 for the first controller, 1 for the second, and so on.  
13. takes in the joystick inputs from a [joysticks object]((https://github.com/MillenniumFalcons/2023-Minibot/blob/master/src/main/java/team3647/lib/inputs/Joysticks.java)) and plugs them into the drive command.

**Tell us when you’re done. If leads aren’t reachable, read this guide before continuing**  
[Falcons explainer](Supplementals/How%20To%20Run%20Falcon%20and%20Kraken%20Motors.md)
