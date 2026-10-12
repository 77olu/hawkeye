Hawkeye

Hawkeye is an autonomous multi-sensor fusion system built in Embedded C on an ATmega328P. It combines light, motion, and temperature data to identify significant sources in a room and automatically point a servo toward the detected target.

Technical Overview

Integrated two phototransistors, an MPU-6050 accelerometer/gyroscope, and a temperature sensor to distinguish between light, heat, and combined events.

Wrote a register-level I2C driver using the ATmega328P's TWI registers, including start/stop conditions, ACK/NACK handling, and MPU-6050 register reads and writes.

Implemented a complementary filter that weights gyroscope and accelerometer data at 98% and 2%, combining short-term responsiveness with long-term stability for tilt estimation.

Compensated the calculated light direction using the system's measured tilt, allowing the servo target angle to remain accurate as the device changes orientation.

Architected the firmware as two FreeRTOS tasks: a sensor task that samples all inputs and a fusion task that processes the readings and controls the servo.

Created a FreeRTOS queue containing structured sensor packets to safely transfer accelerometer, gyroscope, light, temperature, and timing data between tasks every 20 ms.

Configured the ATmega328P ADC at the register level to sample the analog phototransistor and temperature channels without relying on Arduino libraries.

Generated a 20 ms hardware PWM signal with Timer1 to control servo positions from 0 to 180 degrees.

Smoothed the calculated servo angle with a weighted moving update and added a 3-degree deadband to reduce jitter caused by small sensor fluctuations.

Added UART output for live event classification and confidence data, making sensor behavior and fusion decisions easier to test and debug.

Technologies

Embedded C · FreeRTOS · ATmega328P · MPU-6050 · I2C · UART · ADC · PWM · avr-gcc

System Flow

The sensor task samples acceleration, angular velocity, left/right light intensity, and temperature.

A SensorPacket transfers the readings to the fusion task through a FreeRTOS queue.

The fusion algorithm estimates tilt, determines light direction, compensates the target angle, and classifies the detected event.

The filtered angle is sent to the servo when it differs from the current position by more than three degrees.

