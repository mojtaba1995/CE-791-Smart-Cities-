# Waste water platform
Version: 0.5.0<br>
Release date: 2018-03-11<br>
## Summary
This is a cross platform applicationd developed in unity for anlysing specific se<br>

**Goals**
* Capture visual data from its own camera <br>
* Be able to rotate the camera to the needed direction<br>
* Send and receive required data to the mobile base station(HUSKY) using ROS <br>
* Move in outdoor environments of a construction site<br>
* Resist the wind as much as possible<br>
* Land on the mobile base station (HUSKY)<br>
* Resist the winds using tether or motors <br>
# Main Components<br>

## 1. Pan and Tilt 
Pan and tilt mechanism were used in order to rotate and move the camera to the needed direction. Pan and tilt mechanism needs two servos. The servos were connected directly to the raspberry pi GPIO ports. This may reduce the speed of the rotation due to low current in the raspberry pi.<br> 
<br>

## 2. Camera<br>
An Arducam Mini camera module is used to capture image and video data. This camera is mounted on the pan and tilt mechanism so the raspberry pi is able to move its field of view to any needed direction.
The camera module has the following characteristics:
* 5 Megapixel camera <br>
* Power: 1.5V Core, 2.6 - 3.0V Analog, 1.7 - 3.0V I/O <br>
* 5 megapixel (2592×1944): 15 fps (and any size scaling down from 5 megapixel) <br>
* 1080p (1920×1080): 30 fps  <br>
* 720p (1280×720): 60 fps <br>
* VGA (640×480): 60 fps <br>
* QVGA (320×240): 120 fps <br>
<br>
<div align="center">
<img src="https://images-na.ssl-images-amazon.com/images/I/61XA4QT5DQL._SL1000_.jpg" width="350">
</div>
<br>



## 3. Motors<br> 
For altitude, yaw, and forward motion, four DC Hubsan motors were used. Two motors face upwards to control altitude and two motors face forward to control movement and yaw. 
The motors are powered by 3.7 volt LiPo batteries to reduce the current draw from the battery and to ensure that the motors do not overdraw from the Raspberry Pi Zero. 
The motors are controlled by two h-bridge boards that allow the motors to be controlled through pulse width modulation (PWM). Below are the characteristics of the motor:
* The motors can draw a peak 1.2 amps<br>
* They can run up to 5 volts<br> 
* Each motor weighs around 4 grams <br>
* Each motor has a thrust capability of 9 grams <br>
* The thrust to weight ratio of the motor is 0.44 <br>
<br>
<div align="center">
<img src="https://images-na.ssl-images-amazon.com/images/I/41W4c9uP6DL.jpg" width="350">
</div>
<br>


## 4. H-Bridge <br>
Sourcing the motors from the Raspberry Pi could draw too much current from either the Raspberry Pi GPIO pins or the battery. Therefore, 
a h-bridge expansion board and a separate power system were used to ensure that the safe use of the motor system. An Adafruit 
Adafruit TB6612 expansion board was used to ensure drive the motor system. The motor drivers have the following characteristics:<br> 

* Max current draw of 1.2 amps per channel<br>
* Each board can drive 2 DC brushless motors<br> 
* 2.7 - 5.0 V logic, 4.5 - 13.5 V motor (Using 3.7 V LiPo for this system) <br>
* Driven by 3 signals: in1 and in2 for direction and a pwm signal <br>

The following chip was used : [Link](https://www.digikey.com/product-detail/en/adafruit-industries-llc/2448/1528-1212-ND/5353672?WT.srch=1&gclid=Cj0KCQiA5t7UBRDaARIsAOreQtj8RNiJHI8nZwW4rrJkLVZYIzCKm1w7GvDO1qcRLogCcEFeHPWfPFUaAjvmEALw_wcB)
<br>
<div align="center">
<img src="https://www.mouser.com/images/adafruit/lrg/2448_spl.jpg" width="350">
</div>

### Board Schematic and Pin Connections <br>

<br>
<div align="center">
<img src="https://cdn-learn.adafruit.com/assets/assets/000/024/267/original/adafruit_products_schem.png?1427923937" width="350">
</div>


## 5. IMU<br>
An inertial measurement unit (IMU) is an electronic device that measures and reports a body's specific force, angular rate, and sometimes the magnetic field surrounding the body, using a combination of accelerometers and gyroscopes, sometimes also magnetometers.
An Adafruit BNO055 was used for the IMU. The characteristics of the breakout board are given below:<br>

* ARM Cortex M0 Processor<br>
* 9 DOF IMU Sensor (3 DOF Accelerometer, 3 DOF Magnetometer, 3 DOF Gyroscope)<br>
* Embedded temperature sensor<br> 
* Can output filtered absolute orientation, angular velocity, acceleration vector, etc. <br>
* 3.3 - 5.0 V Power Input <br>
* I2C and UART communication interfaces <br>

<br>
<div align="center">
<img src="https://cdn-learn.adafruit.com/assets/assets/000/024/666/large1024/sensors_pinout.jpg?1429726694" width="350">
</div>



## 6. GPS<br>
The Global Positioning System (GPS) is a space-based navigation system that provides location and time information in all weather conditions, anywhere on the outdoor environment. Using this device it is possible to measure coordination and speed of the blimp.
### Code


| Message Type    	| Message Header 	| Frequency 	| Streaming name       	|
|-----------------	|----------------	|-----------	|----------------------	|
| Orientation     	| PoseStamped    	| 100 Hz    	| /OB_imu/pose         	|
| Odometry        	| TwistStamped   	| 100 Hz    	| /OB_imu/twist        	|
| GPS Velocity    	| gps_velocity   	| 10 Hz     	| /OB_gps/general_info 	|
| GPS Info        	| gps_info       	| 10 Hz     	| /OB_gps/general_info 	|
| Teleop commands 	| Twist          	| 10 Hz     	| Blimp_Commands       	|
