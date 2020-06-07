---
layout: post
mathjax: true
title: 'Übung: Conveyor Belt of a Sushi Restaurant'
description: 'Contributor: Donglin Sui'
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="alert('This is the first practice!')">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='ch3.html';">Next</button></span>
</p>

# Introduction

The operation mode of the _conveyor belt/revolving sushi restaurant_ generally goes like this: the sushi chef places the prepared sushi and other Japanese cuisine on plates of different colors and shapes (the color and shape are functional categories used to at the checkout), the chef will then lay these plates on a conveyor belt that passes through all restaurant tables. When those plates entered the sight of diners, they can simply take any plate that suit their appetite off the belt and, enjoy it. At checkout, the bill is settled by the number of plates with weighted factor determined by their color and shape. 

The pros of this form of sushi restaurants are:
* Does not require a large crew of waiters (since there is no need to serve dishes and order food);
* Enhance turnover rate (number of diners served per unit time);
* Friendly to shy and gauche population.

However, the current conveyor belt sushi restaurant faces the following drawbacks, including:
* A gannet diner would severely undermine the experience of other diners, as he would dramatically reduce the 'plate density' on the belt, while there is usually a time lag for the chef to react. Other diners might need to wait for longer time. Moreover, it is hard not to make a mistake at the checkout when you have four dozens or more plates to count;
*  Sushi chef does not have access to prompt update on which sushi is the most consumed and required immediate replenishment. This will lead to a longer waiting time for the diners to grab their favorite eel sushi. It will not only affect the dining experience, but also extend the dining time per unit of diners, and eventually pull down the turnover rate;
*   According to [this research](https://zh.wikipedia.org/wiki/\%E8\%BF\%B4\%E8\%BD\%89\%E5\%A3\%BD\%E5\%8F\%B8), when the conveyor belt is seethed with a sufficient amount of plates, the ideal delivery speed is equivalent to the falling speed of cherry blossoms, that is, five centimeters per second, but when the density of the plates drops, the delivery speed should be appropriately accelerated to eight centimeters per second, so as not to keep the diners in waiting. Current conveyor belt systems generally cannot intelligently adjust the delivery speed in accordance with the varying density of the plates.

In order to solve the aforementioned drawbacks of current systems, this exercise uses ST (structured text) to construct a conveyor belt system that solves the above issues. The program runs on AKENMOTION platform.

# Program Design
## Sushi Movement
In order to be able to simulate the movement of sushi on the HMI (human machine interface), I choose to add the picture of the sushi as an image in Visualization, and then set the variable at the _Movement_ of _Absolute Movement_ in the _image_ menu, as shown below in Figure. 1.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1lIpObCLxJlhlWF4SeBaD0FJ91upWev2M" alt="fig_1.png">
</p>

Create struct in _Application_ for sushi object:
(Warning! If the code block does not render correctly, try to use Chrome and the latest HTML. If it is still not working for you, view  them on GitHub inventory, the code will always be correctly rendered there.)
<details>
  <summary>SUSHI Object</summary>

```
TYPE SUSHI :
STRUCT
    X           :       LREAL;      // X-coordinate of the sushi image
    Y           :       LREAL;      // Y-coordinate of the sushi image
    SUSHI_TYPE  :       INT;        // Sushi types in integer ID
    VISIBLE     :       BOOL;       // TRUE for visible; FALSE for invisible
    DIRECTION   :       STRING;     // 'FORWARD', 'BACKWARD', 'UPWARD', 'DOWNWARD'
END_STRUCT
END_TYPE
```

</details>

In the SUSHI STRUCT, X is used to increment the coordinate in the x-axis direction. N.B. X is not the coordinate but rather the increment value. Y is similarly defined. For instance, when the image sits at $$(-200, 0)$$ at $$t_{0}$$, and SUSHI.X = 75, SUSHI.Y = 100, at $$t_{2}$$ the coordinate of the image will become $$(-125, 100)$$. 'SUSHI_TYPE' keeps the record of the category of cuisine as an integer. Taking modulo of this integer will help us to determined the bill at checkout. VISIBLE is used to record whether the current plate is consumed or not, if consumed, then it is not visible, and this variable will be switched to false. DIRECTION is used to circulate the sushi on the conveyor belt.

### Belt Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1iyxSscqIdF44Ax1D2Mk9xjSSAXEJ9yrm" alt="fig_2.png">
</p>

### Circulation Module
This function circulates the plates on the conveyor belt until diner grabs it.

<details>
    <summary>FUNCTION BLOCK BeltCirculate (VAR Announcement)</summary>
    
```
FUNCTION_BLOCK BeltCirculate
VAR_INPUT
END_VAR
VAR_OUTPUT
END_VAR
VAR
	TON1    :   Standard.TON;
	TON2    :   Standard.TON;
	TON3    :   Standard.TON;
	i       :   INT;          // int var
	ii      :   INT:=0;       // another int var
	speed   :   LREAL;
END_VAR
```

</details>

<details>
    <summary>FUNCTION BLOCK BeltCirculate (Main)</summary>

```
WHILE DoCirculate DO
	TON1(IN:=NOT TON1.Q, PT:=T#0.01S);
	TON2(IN:=NOT TON2.Q, PT:=T#0.05S);
	IF (TON1.Q AND speedLVL) OR (TON2.Q AND NOT speedLVL) THEN
		ii := 0;
		WHILE ii < 6 DO
			IF obj[ii].DIRECTION = 'FORWARD' THEN
				obj[ii].X := obj[ii].X + 5;
				IF obj[ii].X = (1350+ii*400) THEN
					obj[ii].DIRECTION := 'DOWNWARD';
				END_IF
			END_IF
			
			IF obj[ii].DIRECTION = 'BACKWARD' THEN
				obj[ii].X := obj[ii].X - 5;
				IF obj[ii].X = ii*400 THEN
					obj[ii].DIRECTION := 'UPWARD';
				END_IF
			END_IF
			
			IF obj[ii].DIRECTION = 'UPWARD' THEN
				obj[ii].Y := obj[ii].Y - 5;
				IF obj[ii].Y = 0 THEN
					obj[ii].DIRECTION := 'FORWARD';
				END_IF
			END_IF
			IF obj[ii].DIRECTION = 'DOWNWARD' THEN
				obj[ii].Y := obj[ii].Y + 5;
				IF obj[ii].Y = 375 THEN
					obj[ii].DIRECTION := 'BACKWARD';
				END_IF
			END_IF
			ii := ii + 1;
		END_WHILE
	END_IF
END_WHILE    
```

</details>

The conveyor belt is modelled as a pair of coupled electronic gear. The main PLC_PRG is shown below.

<details>
    <summary>PROGRAM PLC_PRG (VAR Announcement)</summary>
    
```
PROGRAM PLC_PRG
VAR
	(* For conveyor belt*)
	fbPower1	:	SM3_Basic.MC_Power;
	fbPower2	:	SM3_Basic.MC_Power;
	fbGearIn	:	SM3_Basic.MC_GearIn;
	fbGearOut	:	SM3_Basic.MC_GearOut;
	fbHalt		:	SM3_Basic.MC_Halt;
	fbMoveVel	:	SM3_Basic.MC_MoveVelocity;
	bStart		:	BOOL;		//Start button for conveyor belt
	bStop		:	BOOL;		//Stop button for conveyor belt
	fbStopRtrig	:	Standard.R_TRIG;
	fbStartRtrig:	Standard.R_TRIG;
	nState		:	BYTE;
	
	(*For moving sushi*)
	
	i			:	INT;					// integer variable
	ii			:	INT;					// another int variable
	iii			:	INT;					// yet another int variable
	TON1		:	Standard.TON; 			// first timer
	TON2		:	Standard.TON; 			// second timer
	fStart		:	BOOL;					// start to put food onto the conveyor belt
	fbBeltCirculate :	BeltCirculate;
END_VAR
```

</details>


<details>
    <summary>PROGRAM PLC_PRG (VAR Announcement)</summary>
    
```
(*Initialization*)
fbPower1(Axis:=AxisX, Enable:= TRUE, bDriveStart:= TRUE, bRegulatorOn:= TRUE);
fbPower2(Axis:=AxisY, Enable:= TRUE, bDriveStart:= TRUE, bRegulatorOn:= TRUE);
readAxisStatus();

(* For conveyor belt*)

FOR i := 0 TO 5 DO
	obj[i].X	:=	0; //-i * 200; // set distance between dishes to be 200 units 
	obj[i].Y	:=  0; // all dishes initially are placed on the same horizontal conveyor belt, namely belt 1
	obj[i].SUSHI_TYPE:= i MOD 3 ; // 3 kinds of sushi in total
	obj[i].DIRECTION := 'FORWARD';	// all sushi initially moving forward
END_FOR


CASE nState OF
	0:
		IF fbPower1.Status AND fbPower2.Status THEN
			nState := 1;
		END_IF
	1:
		fbStartRtrig(CLK:= bStart);
		IF fbStartRtrig.Q THEN
			nState := 2;
			fbGearIn(Execute := FALSE, Master := AxisX, Slave:= AxisY);
			fbMoveVel(Execute := FALSE, Axis:= AxisX);
		END_IF
	2:
		fbGearIn(Execute := TRUE, RatioNumerator := 1, RatioDenominator := 3, Acceleration := 10000, Deceleration:= 10000, Master:= AxisX, Slave:= AxisY);
		fbMoveVel(Execute:= fbGearIn.InGear, Direction:= 1, Velocity := 360, Acceleration:= 10000, Deceleration:= 10000, Axis := AxisX);
		
		(*Circulating the dishes onto the belt*)
		fbBeltCirculate();
		
		fbStopRtrig(CLK:=bStop);
		IF fbStopRtrig.Q THEN
			nState := 3;
			fbMoveVel(Execute:= FALSE, Axis:= AxisX);
			fbHalt(Axis:=AxisX,Execute:=FALSE);
		END_IF
	3:
		fbGearIn(Execute:= FALSE, RatioNumerator:=1, RatioDenominator:= 1, Acceleration:= 10000, Deceleration:= 10000, Master:= AxisX, Slave:=AxisY);
		fbHalt(Axis:=AxisX,Execute:= TRUE, Deceleration:=10000);
		IF fbHalt.Done THEN
			fbHalt(Axis:=AxisX,Execute:=FALSE);
			nState:=4;
		END_IF
	4:
		fbGearOut(Slave:=AxisY,Execute:=TRUE);
		IF fbGearOut.Done THEN
			fbGearOut(Slave:=AxisY, Execute:=FALSE);
			fbHalt(Axis:=AxisY, Execute:= FALSE);
			nState := 5;
		END_IF
	5:
		fbHalt(Axis:=AxisY, Execute:= TRUE,Deceleration:=10000);
		IF fbHalt.Done THEN
			nState:=1;
			fbHalt(Axis:=AxisY,Execute:=FALSE);
		END_IF
END_CASE
```

</details>

To read running state of the axes, we need this readAxisStatus function:

<details>
    <summary>PROGRAM readAxisStatus(VAR Announcement)</summary>
    
```
PROGRAM readAxisStatus
VAR
	fbReadStatusX:	SM3_Basic.MC_ReadStatus;
	fbReadStatusY:	SM3_Basic.MC_ReadStatus;
END_VAR
```

</details>

<details>
    <summary>PROGRAM readAxisStatus (Main)</summary>
    
```
// Display Status of the two Axises
// Status of AxisX
fbReadStatusX(Axis:=AxisX,Enable:=1);
IF fbReadStatusX.Errorstop THEN
	statusX := 'ErrorStop';
ELSIF fbReadStatusX.Disabled THEN
	statusX	:= 'Disabled';
ELSIF fbReadStatusX.Stopping THEN
	statusX := 'Stopping';
ELSIF fbReadStatusX.Homing THEN
	statusX := 'Homing';
ELSIF fbReadStatusX.StandStill THEN
	statusX := 'StandStill';
ELSIF fbReadStatusX.DiscreteMotion THEN
	statusX := 'DiscreteMotion';
ELSIF fbReadStatusX.ContinuousMotion THEN
	statusX := 'ContinuousMotion';
ELSIF fbReadStatusX.SynchronizedMotion THEN
	statusX := 'SynchronizedMotion';
END_IF
// Status of AxisY
fbReadStatusY(Axis:=AxisY,Enable:=1);
IF fbReadStatusY.Errorstop THEN
	statusY := 'ErrorStop';
ELSIF fbReadStatusY.Disabled THEN
	statusY	:= 'Disabled';
ELSIF fbReadStatusY.Stopping THEN
	statusY := 'Stopping';
ELSIF fbReadStatusY.Homing THEN
	statusY := 'Homing';
ELSIF fbReadStatusY.StandStill THEN
	statusY := 'StandStill';
ELSIF fbReadStatusY.DiscreteMotion THEN
	statusY := 'DiscreteMotion';
ELSIF fbReadStatusY.ContinuousMotion THEN
	statusY := 'ContinuousMotion';
ELSIF fbReadStatusY.SynchronizedMotion THEN
	statusY := 'SynchronizedMotion';
END_IF
```

</details>

Finally, set up the following global variables:

<details>
    <summary>VAR_GLOBAL (VAR Announcement)</summary>
    
```
VAR_GLOBAL
	statusX     :   STRING;
	statusY     :   STRING;
	resetDone   :   BOOL;
	obj         :   ARRAY [0..11] OF SUSHI; // array of SUSHI with 11 elements.
	speedLVL    :   BOOL;
	DoCirculate :   BOOL;
END_VAR
```

</details>

# Results

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=11nYxFsCvLna6i_RuADyNq3KaYe7wg1Hb" alt="fig_3.png">
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1UdghSGMzgJ_ovmTy1mzklACjjjltTp7J" alt="fig_4.png">
</p>

***



<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
