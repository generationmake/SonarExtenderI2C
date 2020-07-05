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
