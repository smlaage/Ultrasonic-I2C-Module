# Ultrasonic-I2C-Module
A module comprising 5 ultrasonic distance sensors and I2C interface

This is an experimental module housing 5 ultrasonic distances sensors (HC-SR04) on one board. The sensors are managed by a dedicated ATMega328 which continuously reads the distance measures and makes them available via I2C to a connected host system (typically an Arduino or a Raspberry Pi). The module is intended to support autonomous model cars to find their way without collisions. 

The repository incldues:
- KiCad files with curcuit diagram and PCB layout
- Source code fpr the ATmega328P, developed with ATmel study 7
- Arduino library files including examples
- Fotos

Technical details:
-	5 sensors of type HC-SR04 mounted in different directions (typically (0) left, (1) front left, (2) center front, (3) front right, and (4) right)
-	ATmega238 running at 8 MHZ. The ATmega continuously reads the measurements from all 5 sensors and converts them to approximate mm values. It also runs an I2C slave and makes data available to any host system.
-	I2C interface: The system accepts normal (100 kHz) or fast (400 kHz) clock speeds. The voltage of the interface is 3.3V. It is 5V tolerant.
-	Power: 5V, approximately 30 mA

I2C Interface description:

I2C address: 0x5A (solder jumper open) or 0x5B (solder jumper closed)

Register 0 to 4: returns the distance measurements assigned to the corresponding sensor in mm (16 bit).

Command tokes are submitted as lower nibble. Data for the settings aresubmitted as upper nibble:
- Register 8 (get_mode): returns the current mode (8 bit)
- Register 9 (get_cycle_time): returns the current cycle time (8 bit)
- Register 10 (set_status): stops (upper nibble == 0) or starts (upper nibble > ) the measurements 
- Register 11 (cycle time): specifies the time between triggering the sensors, in 10 ms (upper nibble range 1 to 15). Default value is 3 (= 30 ms). Smaller values give higher speed, however run the risk of overlapping signals. Maximum is 15 (= 150 ms). The value is stored in non-volatile memory and preserved beyond power off.
- Register 12 (mode): Sets the run mode, toggles different settings for the reading of the sensors (upper nibble values 9 ... 3). The value is stored in non-volatile memory and preserved beyond power off.
    - Mode 0 sequence: (0) left -> (2) center front -> (4) right -> (1) front left -> (3) front right.
    - Mode 1 sequence: (0) left -> (2) center front -> (4) right
    - Mode 2 sequence: (1) front left -> (3) front right
    - Mode 3 sequence: (2) center front
