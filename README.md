# Ultrasonic-I2C-Module
A module comprising 5 ultrasonic distance sensors and I2C interface

This is an experimental module housing 5 ultrasonic distances sensors (HC-SR04) on one board. The sensors are managed by a dedicated ATMega328 which continuously reads the distance measures and makes them available via I2C to a connected host system (typically an Arduino or a Raspberry Pi). The module is intended to support autonomous model cars to find their way without collisions. 
Technical details:
-	5 sensors of type HC-SR04 mounted in different directions (typically (0) left, (1) front left, (2) center front, (3) front right, and (4) right)
-	ATmega238 running at 8 MHZ. The ATmega continuously reads the measurements from all 5 sensors and converts them to approximate mm values. It also runs an I2C slave and makes data available to any host system.
-	I2C interface: The system accepts normal (100 kHz) or fast (400 kHz) clock speeds. The voltage of the interface is 3.3V. It is 5V tolerant.
-	Power: 5V, approximately 30 mA

I2C Interface description:

Register 0 to 4: returns the distance measurements in mm (16 bit) assigned to the corresponding sensor.

Register 10 (sensor active): expects an 8 bit value stopping (value == 0) or starting (value > 0) the measurements 

Register 11 (delay time): expects an 8 bit value specifying the time between triggering the sensors, in 10 ms. Default value is 4 (= 40 ms). Smaller values give higher speed, however run the risk of overlapping signals. Maximum is 50. The value is stored in non-volatile memory and preserved beyond power off.

Register 12 (run mode): expects an 8 bit value specifying the run mode. The run mode toggles different settings for the reading of the sensors. The value is stored in non-volatile memory and preserved beyond power off.
Run mode 0 sequence: (0) left -> (2) center front -> (4) right -> (1) front left -> (3) front right.
Run mode 1 sequence: (0) left -> (2) center front -> (4) right
Run mode 2 sequence: (1) front left -> (3) front right
Run mode 3 sequence: (2) center front

