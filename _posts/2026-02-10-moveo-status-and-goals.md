---
layout: post
title: Moveo - status and goals
date: 2026-02-10 04:53 -0700
categories: [robotics, embedded]
tags: [stm32, moveo, firmware]
---

## Motivation and initial choices
Following a class that I took at Colorado School of Mines (ROBO554 - Robot Mechanics: Kinematics, Dynamics, and Control), I felt compelled to put new knowledge into action, after all if I am earning a PhD in robotics, I should probably build at least one robot! So I set out on a project to learn more about controlling a robotic arm.

After looking for 3d printable 6-DoF arms, I settled on [Moveo](https://github.com/BCN3D/BCN3D-Moveo). However, that design was not actually 6-DoF, and I found a project that modified the end effector attachment to include a 6th axis [Moveo modified 6 Axis Robot Arm](https://www.thingiverse.com/thing:2146252/comments). I set about printing.

I was also unsatisfied with the proposed use of Marlin - typically made of 3d printers - as the control interface. So after some research, I decided to write custom firmware for a [BIGTREETECH Octopus](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0). The Octopus is again a 3d printer control board that provides access to 8 stepper drivers! It also uses a STM32F446. While I appreciate everything Arduino has done for the world, I much prefer to program microcontrollers without so much overhead. I have been using STM32s for a couple of years now, so I felt confident that I can make full use of this board. I won't be using 75% of the peripherals on this board, but this route saved me the time and hassle of iterating pcb designs to get something functional, and the Octopus was a decent price. I will be using TMC2209s for the stepper drivers, which means I will configure them through UART, and then controlling motion using the Step/Dir pins.

![Status as of 10FEB2026](/assets/img/moveo_10FEB.JPEG)

## Early challenges
It turns out that late 2025, STMicroelectronics updated their entire framework - which threw me for a loop. I previously enjoyed using CubeIDE with integrated CubeMX for configuring pins and peripherals. ST decided to split the two, which probably makes sense, I also understand that at the same time they improved their support for using VS Code as the development IDE. It took me an entire weekend of debating and experimenting before I was able to compile code and upload it to the Octopus in a easy and reliable manner. I fought through a couple of differnt options:
 - STM32CubeIDE 2.0
 - Platformio
 - Arduino IDE
 - STM32CubeMX CMake in VS Code

With my goal of using the STLINK-V3MINIE as my programming/debugging interface, I settled on the last option. I still need to learn more about CMake to the most of it, but I have come across it enough that I know I should be using it. The V3MINIE has a board-edge connector that exposes everything that I needed, so after crawling through the Octopus schematic, I found the breakouts necessary to program/debug, and also have a serial port for printing information to a terminal on my development computer. The last part will be less important as I get better at using GDB and the debugging tools integrated into VS Code.

![V3MINIE Connections](/assets/img/octopus_v3minie.JPEG)

Once I was able to execute a Hello World program on the Octopus, I tackled setting up the stepper drivers. The TMC2209 I will be using can be configured through UART, however the BIGTREETECH did not wire the stepper drivers to pins on the F446 that include that peripheral. If I were using Arduino, this would be trivally solved by using the built in SoftwareSerial lilbrary - but I am not using Arduino... So I dove back into the documentation of the F446 and CubeMX to figure out how to setup timers. With the timers setup, I was able to implement an interrupt driven bitbang implementation of UART. "I" is a loose term there, I definitely used  ChatGPT to speed the process up.

A challenge that I have using CMake is that there are many great libraries to speed up development - but nearly all of the ones that I am finding use Arduino. To overcome this, I also started to write a thin compatibility layer so that Arduino functions get handled by the STM32 HAL. This is a not a robust system, but it is allowing me to use well tested libraries for the TMC2209, and other sensors that I plan on using as well, but more on those in the future I guess. As of now, my next big task is to add AS5600 digital encoders for each axis. The Moveo hardware was not designed with this in mind, so I might have to re-print some parts. Additionally, the AS5600 has a fixed I2C address... and the octopus only has two I2C ports available... so I will also be using a TCA9548A to multiplex a I2C port.

## Project goals
And now for the whole point of this blog - what are my goals for this project.
 - Finish the 1st axis, I still need to print the motor base and get more timing belt
 - Implement full control of each axis using TMC2209 drivers on the Octopus board using my firmware
 - Add axis position feedback through AS5600s
 - Implement a homing routine
 - Add communication over USB to computer (likely a Raspberry Pi 4) that will do the motion planning
 - Use simple constant acceleration motion planning to move between two poses on command

Now, assuming I get to this point, any future development would be software only, so this seems like a good potential pause point, or off ramp if necessary.