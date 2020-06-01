# SonarExtenderI2C
Connect up to four Ultrasonic sensor HC-SR04 via I2C to an Arduino or Raspberry Pi

### Upper 8 bit
Bit 7: trig US3\
Bit 6: trig US2\
Bit 5: trig US1\
Bit 4: trig US0\
Bit 3: status Echo\
Bit 2: decode Sense 2\
Bit 1: decode Sense 1\
Bit 0: decode Sense 0

### lower 8 bit
Counter register = proximity

## Automatic ultrasonic sensor evaluation

There is a mode for automatic evaluation of all sensors that can be turned on and off using the decode pins.
The auto evaluator is based on a 4 bit Johnson ring counter with 10 Hz clock frequency. If automatic mode is started, the counter will be preset by a d-latch. To prevent wired or issues, also Q3 from the counter latch is ored to the preset circuit. This part of the circuit is realized by a 74xx175 and 74xx00 ic. The or gate is replaced by nand gates to reduce the amount of chips, as the Auto signal also needs to be inverted. After all, the actual ring counter will be preset to Q0 = 1 if Auto mode is selected or Q3 of the counter is high ( -> endless counting ). Also the master reset of the counter ic is connected to auto, so it can be turned off again. 
With every counting cycle one ultra sonic sensor will be triggered and counter enabled/ other counters stopped. 
The outputs of the automatic circuit will be coupled with the rest of the circuitry using a nand and one and gate.
