# Step 3: minibot turn wheel 90

Now it’s time to move on to PID. In this stage, you’ll program the wheel of the minibot to turn to 90 degrees. You’ll be using the PIDController class to run the loop.   
**PID controller background:** 

1. Initialize a PID controller with the constants of 0,0,and 0 for your P I and D.   
2. The PIDController class has a method called calculate(measurement, setpoint) that will calculate the desired output from a measurement and the desired ending state.

**Using the PID controller:**

3. Get the current position of the motor that you want to move  
4.  Then use the calculate method and plug in the current position of the motor and the position you want to go to (the setpoint).   
5. Set the motor output to what the calculate() method returns.   
	1. Keep in mind, you are trying to set the position of the *wheel* in *degrees.* The Spark Max reads the position of the *motor* in *rotations.*   
	2. The minibot has gearing, so for every wheel rotation, the motor rotates 5 times.   
	3. You must convert from rotations of the motor to degrees of the wheel, and have a method to set the wheel rotations in degrees

**More PID controller theory:**

6. This is the formula for the PID controller: kP \* e(t) \+ kI \* ∫e(t) \+ kD \* e’(t)  
	1. **If you aren’t familiar with calculus, don’t worry about the kI and kD parts yet**  
	2. The e(t) is the graph of the error (setpoint \- measurement or goal \- input) of your system  
	3. kP, kI, and kD are constants, that serve as sort of weights for each portion of the calculation  
	4. Notice that setting any constant to 0 effectively removes that part of the calculation from the equation. 

8. Let’s walk through a scenario where you’re trying to control position, let's say setting it to 5 meters, and you only directly control motor voltage  
	1. We’ll start with PID gains of 1,0,and 0 for kP, kI and kD respectively  
	2. At the start, the error is 5, and the only thing that is used is the kP component, so the output voltage  will be 1 \* 5  \= 5 volts  
	3. Let’s say that after some time, we are now at 3 meters  
		 1. The error is now 2, and the output voltage is therefore 2  
	4. In general, as we get closer to the setpoint, the output voltage should decrease, leading to smooth motion

10. The above example is an ideal situation with no delay. In truth, the robot code runs only once every 20ms, which is plenty of time for the system to move before the code can measure and react to it.  
	1. So, if your kP gain is too high, you will start to see oscillations, where the system continually overshoots and overcorrects for it  
	2. These oscillations are generally dampened by adding some kD component, which will slow down the output more the closer it gets to the setpoint

**Tuning a PID controller:**

9. From all the theory above we can come up with a process for tuning a PID controller  
	1. [Binary search](https://www.youtube.com/watch?v=MFhxShGxHWc) for the closest-to-perfect P value that you can get  
		 1. If you see oscillations, the P is too high, and if you don’t get to your setpoint, the P is too low  
	2. If you still can’t control oscillations, or if you can’t get it accurate enough, increase P till you see some oscillations, then add a small amount of D to dampen them  
	3. In general, don’t touch I, since it will lead to weird behavior, and won’t really help anything unless you’re trying to go a long distance
 10. For practice tuning a pid controller, you can see [this website](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/tuning-turret.html#pure-feedback-control)

 > [!IMPORTANT]  
 >**Your Project:**
>* you will create a command in your existing minibot code that will turn one (or both) of the wheels of the minibot by using a `PIDController`
 
Here’s how the structure should go in code:

* There’s a setAngle() method in your subsystem that should set the motor output variable  
  * This should allow you to set the angle to any degree value  
* Then, there’s a command that calls the setAngle() method for 90 degrees  
* Remember to bind your command to a button
