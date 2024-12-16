
## odometry
It all starts with [odometry](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/differential-drive-odometry.html) . So what do you need for odometry?
1. Differential Drive Odometry, which in turn requires the following:
	1. a starting pose
	2. functions to find the left and right distances (this isnt passed into the constructor)
	3. starting distances for left and right (this is passed in), should be 0 for both sides
	4. starting gyro rotation (just call the gyro rotation, but it should be 0 when code boots)
2. To update odometry, call update() every loop. requires:
	1. gyro angle (rotation2d)
	2. current left distance (m)
	3. current right distance (m)
3. If you dont have akit set up yet, you should do that now (need add link for aktit tutorial)
4. log your pose (returned by the update method to get pose)
## adding vision
now, we need a different type of class, the [DifferentialDrivePoseEstimator](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/state-space/state-space-pose-estimators.html).  it works pretty much the same way as the odometry class, but it has methods to add vision pose. It requires an extra argument
-  [differential drive kinematics](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/DifferentialDriveKinematics.html) 
	- give it your trackwidth (distance from right to left wheel) 
use this the same way as odometry, calling update every loop. there area also methods to add a vision pose when you get one 
### adding a vision pose
What you need: 
- the pose (obviously)
- the timestamp that the pose was taken 
	- for limelight, use current timestamp - latency (get it from limelight helpers)
    - for Photonvision, the synced timestamp is already provided
-  the standard deviation for the pose
	- this tells the pose estimator how much to trust the x, y and theta that the vision pose gives us
		- generally the x and y are small numbers (.05) that are scaled by the distance to the tag
		- the theta deviation is usually high bc we have a gyro and that's much more correct
	- the stdev is a ```vector<N1,N3>``` and is filled by ```Vecbuilder.Fill(xDeviation, YDeviation, ThetaDeviation)``` 
once you have all these, call ```addVisionMeasurement()``` to add the data