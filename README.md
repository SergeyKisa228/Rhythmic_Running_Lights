# Rhythmic_Running_Lights
Compact sound‑reactive LED controller based on STM32F103C8T6 and KY‑037 microphone module. Two operation modes – random flash to the beat or classic running lights – switchable by a button.

<img width="1280" height="760" alt="2" src="https://github.com/user-attachments/assets/de6f4c39-8110-42e7-ad88-a77ff74dfd45" />

# Description :)

The device listens to ambient sound (music, claps, beats) via the digital output of the KY‑037 sensor.

When a loud sound is detected, the microcontroller instantly updates the pattern of 8 LEDs driven by a 74HC595 shift register.

Mode 0 (default) – each beat produces a random 8‑bit pattern; LEDs remain lit until silence >80 ms, then they turn off.

Mode 1 – classic running light with fixed speed (150 ms per step).

A push button (PB1) toggles between modes and provides visual feedback (PC13 blinks 1 or 2 times).

Built‑in debouncing for both the microphone and the button ensures stable operation.

All components are powered from a single MB102 breadboard power supply (5 V rail).

<img width="640" height="368" alt="26" src="https://github.com/user-attachments/assets/eb8506a2-2fd1-47d0-bc93-4efe498dc873" />

# Hardware 🔧

-STM32F103C8T6 (Blue Pill)

-KY‑037 sound sensor (only DO used – digital output)

-74HC595 8‑bit shift register

-8× LEDs + 8× 220 Ω resistors

-Button (PB1, pull‑up)

-MB102 breadboard power supply (5 V)

-Breadboard, jumper wires

# Schematic ❗
<img width="583" height="588" alt="Scematic" src="https://github.com/user-attachments/assets/759d6976-a6bc-4045-9820-0b1908e905e1" />

# How it works 💬

Initialisation – system clock 72 MHz (HSE 8 MHz), GPIO, EXTI for PB0, random seed from HAL_GetTick().

Main loop – two parallel paths:

Mode 0 – when beat_dec is set by the EXTI callback:

Generate random byte → send to 74HC595.

Blink PC13 briefly.

After 80 ms without a beat, send 0x00 to turn LEDs off.

Mode 1 – cycle a single 1 through all 8 outputs with 150 ms delay.

Button handling – polled every cycle with 200 ms debounce.

Short press toggles state between 0 and 1, calls flash_led(state+1) and sends 0x00 to reset LEDs.

Error resilience – the interrupt routine filters glitches; main loop never blocks

https://github.com/user-attachments/assets/6cd7e8c8-903c-4c3d-8483-3a86d7764738

## Author
👾 **SergeyKisa228** 👾
