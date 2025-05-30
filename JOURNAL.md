---
title: "Highway Helper"
author: "Allen"
description: "Display that reflects off the windshield to display speed, dangers, etc"
created_at: "2025-05-28"
---
# May 30 - Mostly Planning
I mostly just planned out what exactly my goals were and started looking at components. While a RockChip SOM board would be more fun to design around, they are hard to source, mostly being from AliBaba with expensive but slow shipping. However, I found the Radxa Rock 4D, which seems super powerful but is only $45, which is a very good deal for these kinds of AI SBCs. It also has a bunch of peripheral ports such as MIPI for cameras and displays, and CAN support. I think I will use a MCP2551 CAN transciever for the OBD2 connection, as it has silent mode to lower the risk of interfering with the car's operation. I also found the CH224K USB-PD driver, which should be able to convert the 12V of a car to the correct voltage to power the processor. However, I want to find a way to power it through the GPIO header because it is a lot cleaner that way, with less external wires and everything being on the same PCB.
![image](https://github.com/user-attachments/assets/1db299be-dbe5-41ad-bae1-005005b36d4f)

I also started working on the schematic: 
![image](https://github.com/user-attachments/assets/51b84483-6c85-45c2-9a79-458685378a80)
**Total time spent: 1.5h**
