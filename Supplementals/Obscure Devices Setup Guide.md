# ADIS gyro (minibot gyro)

to intialize, use the built in `ADIS16470_IMU` class. Use the default constructor with no arguments


To get the angle, use the `getAngle()` method to get the yaw in degrees


# DogLog
1. download the DogLog vendor library from the panel in wpilib
2. to use it do `DogLog.log(String key, <whatever object> value)`
	1. the object you try to put in must be a primitive, string, or struct
	2. baiscally, you can log certain things like poses but not *all* objects
	3. if there's no function to log that specific object, you're cookd