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

### Manual mode

#### 1. Initiate trigger signal 

Before every manual measurement make sure, that P4-P7 (TRIGGER0-TRIGGER3) are low and P0-P2 (DECODE0-DECODE2) are smaller than 4 in binary.
In manual mode You can trigger a sensor by setting P4-P7 (TRIGGER0-TRIGGER3) high according to which sensors You want to use:

|          | P4 | P5 | P6 | P7 |
|----------|----|----|----|----|
| Sensor 0 | H | L | L | L |
| Sensor 1 | L | H | L | L |
| Sensor 2 | L | L | H | L |
| Sensor 3 | L | L | L | H |

#### 2. Wait for measurements to finish

If all measurements are finished P3 (STATUS ECHO) will go high. Here it is important to wait a minimum of 100us after triggering the sensor.

#### 3. Evaluate the measurement result

The sensorboard has a 8-bit counter for every sensor, that is started and stopped automatically according to the measurement cycles. You can read the data on P10-P17 by setting the decode pins P0-P2 to the right counter:

|          | P0 | P1 | P2 |
|----------|----|----|----|
| Sensor 0 | L | L | L |
| Sensor 1 | H | L | L |
| Sensor 2 | L | H | L |
| Sensor 3 | H | H | L |

As the counters are running at a 10kHz frequency and the ultrasonic signal needs 29.4us to travel one centimeter, You can calculate the distance to an object in cm by the following formular: ```distance = (countervalue/2)/29.4```.

### Automatic mode
