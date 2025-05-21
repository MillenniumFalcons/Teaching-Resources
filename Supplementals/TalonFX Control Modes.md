# TalonFX Control Modes


| Type of Control                                    | Voltage Control            | Torque Control                     | percentage Output (Proportional to Voltage) |
| -------------------------------------------------- | -------------------------- | ---------------------------------- | ------------------------------------------- |
| Raw (no PID Tuning Requried)                       | VoltageOut                 | TorqueCurrentFOC                   | DutyCycleOut                                |
| Velocity                                           | VelocityVoltage            | VelocityTorqueCurrentFOC           | VelocityDutyCycle                           |
| Position                                           | PositionVoltage            | PositionTorqueCurrentFOC           | PositionDutyCycle                           |
| Motion Magic (Profiled Velocity)              | MotionMagic(Expo)Voltage   | MotionMagic(Expo) TorqueCurrentFOC | MotionMagic (Expo)DutyCycle                 |
| Motion Magic Velocity (Profiled Acceleration) | MotionMagicVelocityVoltage | you get the idea                   |                                             |




## Profiled
- Profiling fits the derivative of the control mode to a curve, so you reach the setpoint smoothly
#### Position
If we're trying to control Position, we can profile the velocity, so the position and velocity values might look like this:


<Img src = https://v6.docs.ctr-electronics.com/en/stable/_images/trapezoidal-profile.png>

As you can see, the velocity is controlled, and the position reaches the setpoint smoothly


#### Velocity
The same applies for Velocity, except we profile acceleration, and make velocity reach the setpoint smoothly

On the graph, the blue line becomes acceleration, and the red line becomes velocity

