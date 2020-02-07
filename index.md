---
layout: default
mathjax: true
title: 'PLCopen Notes'
description: Logon didonai
---
The notes here are taken for the motion control standard _PLCopen IEC61131-3_ during my internship @Gatherwin. These notes serve as a reminder and quick review material for future use.


## **Table of Content**

* **Kapitel I: What is PLCopen and IEC61131-3?**
* [**Kapitel II: Motion Control Theory**](Kap02MCT.html)
* [**Kapitel III: Exercises**](Kap03Exercise.html)

***
# What is PLCopen and IEC61131-3?

Imagine you're a butler serving the Grandet family. Been Grandets, they hire no valet nor footman to share your load and expect you to do all the household chores. You are smart (but perhaps not that smart, judging from your choice of career), so you decide to automate some of your tasks using machines. You remember that once you were cooking dinner, and then the Grandets went back from the _Miser's Salon_. They rang the bell and waited at the door, as apparently they would never condescend to open it. You heard the bell, so you had to put down all the work on your hands, turned off the stove, and ran to answer the door. Afterwards, they haughtily blamed you for taking too long. This experience is traumatizing, therefore you decided to start from here.

You purchased a motor (using your own salary, of course), installed it onto the door and wired up a switch in the kitchen so you can remotely control the door. You also placed a video camera outside the front door so the Grandets do not need to ring the bell. The system performed well, and the Grandets stopped grumbling about your untimley answering. You decided to extend the system to other chores, like opening the window, pulling the curtain and adding firewoods to the hearth. These systems worked very well and help you to reduce a lot of work. It's just that your kitchen is now full of flying wires, screens and dashboards. 

![Control panel in the kitchen](assets/images/control_panel.jpg)

So you bought a microcomputer to gather these control circuits. At the same time, you realized that you can use some simple programs to  perform some operations, such as when the temperature sensor detects a temperature drop of the fireplace, it will automatically activate the nearby mechanism to add firewood. In order to do so, you have to learn the programming language adopted by the chip manufacturer first. 


> PLCopen was founded in 1992 just after the world wide programming standard IEC 61131-3 was published. The controls market at that time was a very heterogeneous market with different types of programming methods for many different PLCs. The IEC 61131-3 is a standard defining the programming languages for PLCs, embedded controls, and industrial PCs, harmonizing applications independent from specific dialects, but still based on known methods such as the textual programming languages Instruction List, and Structured Text, the graphical programming languages Function Block Diagram and Ladder Diagram (a.k.a. Ladder logic), and the structuring tool Sequential Function Chart.
> - [_Wikipedia_](https://en.wikipedia.org/wiki/PLCopen)

