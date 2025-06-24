---
title: "Puyallup Control Hub"
author: "Allen"
description: "Be a standalone device capable of controlling a robot with motors, sensors, cameras, etc"
created_at: "2025-05-28"
---
# June 6
I decided on the Allwinner T527 because it is super cheap at like $15 and for some reason has 8 Cortex-A55's which is like really good but I'm kind of sad that the one on LCSC doesn't have an NPU which I wanted to play with. I will have 2GB of LPDDR4 RAM which should be enough.
I started wiring the RAM this is so cooked
![image](https://github.com/user-attachments/assets/eff2913d-4ac9-4881-8673-a19fe9d45f2a)

Time Spent: 2h

# June 23
Apologies, I have kind of been working on this on and off in my free time after school and not really journaling. School is over now and I have all the free time in the world, I guess I will catch up now.

I decided to switch to the RK3562 because I realized that Allwinner chips have like no software support and that made me really sad. I am instead switching to the RK3562 that has 4 Cortex-A53's which is very similar to the RK3566 but more efficient and I might be able to adapt existing code. 

New RAM (I figured out how to use KiCad buses which makes it a lot cleaner.
![image](https://github.com/user-attachments/assets/cdecc7b6-11ce-475a-8bae-c0f415499f2d)
![image](https://github.com/user-attachments/assets/62661868-a07d-4bbf-af4c-dddd4659f48c)

SD Card + Wireless chip in progress. I first wanted to use the AP6236 which is used on a lot of other boards but it is kind of expensive at $5 and also pretty bulky, but I found the RTL8723 which does not have a built in antenna but an external antenna is probably better anyway.
![image](https://github.com/user-attachments/assets/26a7f900-00c9-4208-8d84-984b7cf6ce14)

As this is supposed to be a robot control hub, I am also figuring out how to add motor controllers onboard. Ideally, these would be hybrid motor ports that allow for both brushed and brushless DC motors to plug in. I am planning on using the STM32G431 as a coprocessor for the motor controller, and G2061Q gate driver. I still need to decide on what MOSFETs to use because I have found some pretty tiny ones, but I need 6 channels for each motor.

![image](https://github.com/user-attachments/assets/83e59d8d-cbdb-430c-aaa2-11d1681d9f50)

This is the oscillator circuit
![image](https://github.com/user-attachments/assets/9ca34551-c192-4c3d-ac54-1661ab96f6e9)

On I2C Bus 0, I plan on having all the internal sensors and communication. This will include the LSM6DS3 IMU, and other stuff. I don't know if I want to use I2C or UART or what to communicate between expansion ports. The REV Control hub uses half-duplex RS485 which is super slow especially during I2C because of a lack of parallelization and only having one sided communication at a time. I hope to avoid that, and I also don't want to do anything with differential pairing stuff because it is annoying to wire, so no CAN. SPI might work but limited chip selects would be a problem if I make an entire ecosystem or something (I guess UART would be cooked too).

![image](https://github.com/user-attachments/assets/ddb0c5f6-021b-4684-8897-b2a7376bbb87)

Here is the power management schematic stuff, I just followed the voltages from the datasheet although I may need to look into isolation and decoupling idk how it really works.

![image](https://github.com/user-attachments/assets/a847bed2-944d-4fa3-ab30-b0ba56bb7c40)

I decided to use the RT9992 power management IC, because while the RK808 seems like a popular choice, I feel like it would be super annoying to program the I2C to even get the voltage to output correctly. Instead, this chip just selects voltage by putting resistors and capacitors at the configuration pins.

![image](https://github.com/user-attachments/assets/f892bca9-d308-487b-bb61-a372d2a1e324)

Time Spent: 5h

**TOTAL TIME SPENT: 7h**
