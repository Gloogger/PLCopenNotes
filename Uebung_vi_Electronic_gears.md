---
layout: post
mathjax: true
title: 'Electronic Gears'
description: Implementation of electronic gears using ST language
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="alert('This is the first chapter!')">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='ch3.html';">Next</button></span>
</p>

# What is Electronic Gear?
In pure mechanical systems, the rotational speeds of two axes can be adjusted by a set of gears. The drawbacks of such design are:

1. Once set up, the speed ratio between the axes is fixed. It is difficult and expensive to adjust the speed ratio, because the old gears may need to be replaced.
2. Takes up room.
3. Induces mechanical losts and generates noise.

One way to overcome the aforesaid disadvantages is to replace the pure mechanical system with two motors controlled by a PLC. The speed ratio between the two motors is defined by the onboard program and can be easily modified.

***

# Planning
![flow chart](assets/images/flow_chart_electronic_gear.jpg)

# Code
'''reStructuredTest
PROGRAM PLC_PRG
VAR
	fbPower1, fbPower2 	:	SM3_Basic.MC_Power;
	fbHalt		:		SM3_Basic.MC_Halt;
	fbStartRtrig, fbStopRtrig 	:	Standard.R_TRIG;
	fbMoveVel	:	SM3_Basic.MC_MoveVelocity;
	fbGearIn	:	SM3_Basic.MC_GearIn;
	fbGearOut	:	SM3_Basic.MC_GearOut;
	
	nState		:	INT;
	myEnable, startFlag, stopFlag :	BOOL;
	fbStop1, fbStop2:	SM3_Basic.MC_Stop;	
	
	gearRatioNumerator	:	Uint;
	gearRatioDemonimator:	UINT;
END_VAR
'''

# Summary

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
