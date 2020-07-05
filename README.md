# SonarExtenderI2C
Connect up to four Ultrasonic sensor HC-SR04 via I2C to an Arduino or Raspberry Pi

![SonarExtender rendering](/doc/pcbrendering.png)

## Connection

### Ultrasonic sensor connection (CN3-CN6)

| Pin | Function |
|-----|----------|
| 1 | 5V |
| 2 | Trigger |
| 3 | Echo |
| 4 | GND |

### Mikrocontroller connection (P1)

| Pin | Function |
|-----|----------|
| 1 | GND |
| 2 | SCL |
| 3 | SDA |
| 4 | Logic voltage (3V3/ 5V) |
| 5 | Interrupt pin |
| 6 | Ultrasonic sensor voltage (5V) |

## How to use

The sensorboard is based on the 16-bit I2C Expander TCA9535 with the slave address 0x20. There are two operating modes for the device. In the man ual mode You can evaluate every sensor on Your own, in the automatic mode, all four sensors are evaluated sequentially and an interrupt is rising when all four sensors are triggered once.

### TCA9535 pinout

| Pin | Function |
|-----|----------|
| P0 | DECODE0 |
| P1 | DECODE1 |
| P2 | DECODE2 |
| P3 | STATUS ECHO |
| P4 | TRIGGER0 |
| P5 | TRIGGER1 |
| P6 | TRIGGER2 |
| P7 | TRIGGER3 |
| P10 | D0 |
| P11 | D1 |
| P12 | D2 |
| P13 | D3 |
| P14 | D4 |
| P15 | D5 |
| P16 | D6 |
| P17 | D7 |

### Manual mode

#### 1. Initiate trigger signal 

Before every manual measurement make sure, that TRIGGER0-TRIGGER3 are low and DECODE0-DECODE2 are smaller than 4 in binary.
In manual mode You can trigger a sensor by setting TRIGGER0-TRIGGER3 high according to which sensors You want to use:

|          | TRIGGER0 | TRIGGER1 | TRIGGER2 | TRIGGER3 |
|----------|----|----|----|----|
| Sensor 0 | H | L | L | L |
| Sensor 1 | L | H | L | L |
| Sensor 2 | L | L | H | L |
| Sensor 3 | L | L | L | H |

#### 2. Wait for measurements to finish

If all measurements are finished STATUS ECHO will go high. Here it is important to wait a minimum of 100us after triggering the sensor.

#### 3. Evaluate the measurement result

The sensorboard has a 8-bit counter for every sensor, that is started and stopped automatically according to the measurement cycles. You can read the data on D0-D7 by setting DECODE0-DECODE2 to the right counter:

|          | DECODE0 | DECODE1 | DECODE2 |
|----------|----|----|----|
| Sensor 0 | L | L | L |
| Sensor 1 | H | L | L |
| Sensor 2 | L | H | L |
| Sensor 3 | H | H | L |

As the counters are running at a 10kHz frequency and the ultrasonic signal needs 29.4us to travel one centimeter, You can calculate the distance to an object in cm by the following formular: ```distance = 100*countervalue/2/29.4```. The maximum distance is 425cm with an accuracy of 4.25cm.

### Automatic mode
