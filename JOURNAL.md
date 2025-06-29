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

# June 28

I spent a lot of time researching power management and decoupling and I started working on it.

Here's my chain of decoupling capacitors for every single VDDQ pin on the RAM (many many more coming soon) 

![image](https://github.com/user-attachments/assets/a1504b5e-b6b5-476d-b270-9c7eb9104e16)

Also started working on the feedback stuff for the RT9992 buck conversion with a very cool 22 milliohm inductor I found on LCSC (also one of the cheapest 2.2uH inductors). The buck converters are pretty interesting and they seem complicated but they are actually pretty simple, you just have to create a voltage divider at the feedback pin to change the voltage. That is pretty cool.

![image](https://github.com/user-attachments/assets/a6fda9e8-c734-4aad-929f-4f7a6b896ccc)

Time Spent: 2h

# June 29

I kept working on the power management, and I realized that this is actually pretty complex because I want to be able to support both a 3.7V LiPo battery as well as a larger 12V battery that would be used in FTC. However, this RT9992 only works with up to 7V, (although the channel 6 power apparently accepts up to 21V???). I plan on adding another step down converter to get the 12V down to like 4V. I need it to be below 5V because I want 5V output and I am running out of step down channels so it needs to be step up.

Channel 2 has been added. I changed channel 1 to the 5V because I realized it is step-up only. Channel 2 needs to be 0.9V which follows the formula in the datasheet, Vout = (1+R8/R9) * 0.8V. I don't know what capacitors to use because the datasheet lists capacitor values that seem to change nonlinearly with the voltage output. I must understand more.

![image](https://github.com/user-attachments/assets/1b5b3b45-ede0-41ea-96da-bb4e082e705b)

More decoupling!

![image](https://github.com/user-attachments/assets/6e4c653b-e7b2-4201-b27b-a56b38007e34)

Also a switch between the 3.7V battery, nothing, and the 12V battery. Problem is, if they are both assumed to be 3-4V at the same place, the step down converter for the big battery would always be powered which I don't want (although the output would have zero load so idk how much it actually matters).

![image](https://github.com/user-attachments/assets/ffb54c98-84e9-43ae-b2d4-a5314cc8f27a)

Time Spent: 30m

**TOTAL TIME SPENT: 9.5h**
