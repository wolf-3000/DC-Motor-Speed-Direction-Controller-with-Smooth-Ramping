# DC-Motor-Speed-Direction-Controller-with-Smooth-Ramping

## Overview

This Arduino sketch drives a DC motor through an H-bridge (e.g. L298N), with:

- Potentiometer-controlled target speed (0–255 PWM)
- A 5-sample moving average filter to smooth the analog reading
- Non-blocking, `millis()`-based speed ramping (acceleration/deceleration)
- Two-button direction control (forward / reverse)
- Live status reporting over Serial

## Hardware

| Component              | Notes                          |
|------------------------|--------------------------------|
| Arduino Nano (or Uno)  | Main controller                |
| L298N motor driver     | H-bridge for DC motor          |
| DC gear motor          | Driven load                    |
| 10k potentiometer      | Speed setpoint, connected to A0 |
| 2x pushbutton          | Direction control, `INPUT_PULLUP` |
| Power supply           | Sized for your motor + driver  |

## Pinout / Wiring

| Arduino Pin | Connected To       | Purpose          |
|-------------|---------------------|------------------|
| 3 (PWM)     | Driver ENA          | Motor speed      |
| 4           | Driver IN1          | Direction bit 1  |
| 5           | Driver IN2          | Direction bit 2  |
| A0          | Potentiometer wiper | Speed setpoint   |
| 6           | Button 1 → GND      | Drive forward    |
| 7           | Button 2 → GND      | Drive reverse    |

## Build Photos

<img width="1496" height="1496" alt="piccc_2" src="https://github.com/user-attachments/assets/20d9941a-5e41-40b3-9f33-7fbd34eb355d" />
<img width="1496" height="1496" alt="piccc_1" src="https://github.com/user-attachments/assets/a5b9453f-052e-4e86-8045-b5bb1e81a0dc" />

## How It Works

1. `smoothRead()` reads the potentiometer, maps it to 0–255, and runs it through a 5-sample rolling average to filter ADC noise.
2. `isButtonPressed()` polls each button every 50 ms as a simple time-based debounce, without blocking the main loop.
3. `updateMotorSpeed()` nudges `currentSpeed` toward `targetSpeed` by ±1 every 20 ms, producing a gradual ramp rather than an instant jump in PWM duty cycle.
4. Button 1 sets direction "forward" (`IN1=HIGH, IN2=LOW`), button 2 sets "reverse" (`IN1=LOW, IN2=HIGH`).
5. `printDashboard()` prints target speed, current speed, and ramp state to Serial every 500 ms.

e]
