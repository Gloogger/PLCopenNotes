---
layout: post
mathjax: true
title: 'Kapitel Nummer'
description: Einige Beschreibung hier
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='übung05_parkingStation.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="alert('This is the last exercise!')">Next</button></span>
</p>

# Program Design
## Device Hierarchy
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1391ssNk652hznTOPWvWNZs8IfRp0SW6S" alt="fig_1.png">
</p>

## PLC_PRG
<details>
    <summary>VAR Declaration</summary>
    
```
PROGRAM PLC_PRG
VAR
	//红绿灯启动开关
	power: BOOL;
	reset: BOOL;
	//东，南，西，北红绿灯开关（E代表东，S代表南，N代表北，W代表西）
	EWGreenLit: BOOL;
	EWRedLit: BOOL;
	EWYellowLit: BOOL;
	EWLeftGLit: BOOL;
	EWLeftYLit: BOOL;
	EWLeftRLit: BOOL;
	hideEWLeftGLit: BOOL;
	hideEWLeftYLit: BOOL;
	
	NSGreenLit: BOOL;
	NSRedLit: BOOL;
	NSYellowLit: BOOL;
	NSLeftGLit: BOOL;
	NSLeftYLit: BOOL;
	NSLeftRLit: BOOL;
	hideNSLeftGLit: BOOL;
	hideNSLeftYLit: BOOL;
	
	EWStraight: BOOL:= TRUE;
	EWLeft: BOOL:= TRUE;
	NSStraight: BOOL:= TRUE;
	NSLeft: BOOL:= TRUE;
	
	wait: TON;
	wait1: TON;
	nState: BYTE;
	
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
IF reset THEN
	EWGreenLit:= FALSE;
	EWRedLit:= FALSE;
	EWYellowLit:= FALSE;
	EWLeftGLit:= FALSE;
	EWLeftYLit:= FALSE;
	hideEWLeftGLit:= FALSE;
	hideEWLeftYLit:= FALSE;
	
	NSGreenLit:= FALSE;
	NSRedLit:= FALSE;
	NSYellowLit:= FALSE;
	NSLeftGLit:= FALSE;
	NSLeftYLit:= FALSE;
	hideNSLeftGLit:= FALSE;
	hideNSLeftYLit:= FALSE;
	
	EWStraight:= TRUE;
	EWLeft:= TRUE;
	NSStraight:= TRUE;
	NSStraight:= TRUE;
	nState:= 0;
END_IF

CASE nState OF
	0:
	IF power THEN
		nState:= 2;
	//ELSE
		//nState:= 1;
	END_IF
	
	(*1:
	EWGreenLit:= FALSE;
	EWRedLit:= FALSE;
	EWYellowLit:= FALSE;
	EWLeftGLit:= FALSE;
	EWLeftYLit:= FALSE;
	hideEWLeftGLit:= FALSE;
	hideEWLeftYLit:= FALSE;
	
	NSGreenLit:= FALSE;
	NSRedLit:= FALSE;
	NSYellowLit:= FALSE;
	NSLeftGLit:= FALSE;
	NSLeftYLit:= FALSE;
	hideNSLeftGLit:= FALSE;
	hideNSLeftYLit:= FALSE;
	
	EWStraight:= TRUE;
	EWLeft:= TRUE;
	NSStraight:= TRUE;
	NSStraight:= TRUE;
	nState:= 0;*)
	
	2:
	hideEWLeftGLit:= FALSE;
	hideEWLeftYLit:= FALSE;
	hideNSLeftGLit:= FALSE;
	hideNSLeftYLit:= FALSE;
	EWGreenLit:= TRUE;
	EWLeft:= FALSE;
	nState:= 3;

	3:
	wait(In:= TRUE, PT:= T#10S);
	IF wait.Q THEN
		wait(In:= FALSE);
		EWGreenLit:= FALSE;
		nState:= 4;
	END_IF
	

	4:
	wait(In:= TRUE, PT:= T#3S);
	wait1(In:= TRUE, PT:= T#0.5S);
	IF wait1.Q THEN
		EWYellowLit:= NOT EWYellowLit;
		wait1(IN:= FALSE);
	END_IF

	IF wait.Q THEN
		wait(IN:= FALSE);
		EWYellowLit:= FALSE;
		EWLeft:= TRUE;
		EWRedLit:= TRUE;
		EWLeftGLit:= TRUE;
		NSStraight:= FALSE;
		nState:= 5;
	END_IF
	

	5:
	wait(In:= TRUE, PT:= T#5S);
	IF wait.Q THEN
		wait(IN:= FALSE);
		EWLeftGLit:= FALSE;
		hideEWLeftGLit:= TRUE;
		nState:= 6;
	END_IF
	
	
	6:
	wait1(IN:= TRUE, PT:= T#0.5S);
	wait(IN:= TRUE, PT:= T#3S);
	IF wait1.Q THEN
		EWLeftYLit:= NOT EWLeftYLit;
		wait1(IN:= FALSE);
	END_IF

	IF wait.Q THEN
		wait(in:= FALSE);
		EWLeftYLit:= FALSE;
		hideEWLeftYLit:= TRUE;
		EWLeftRLit:= TRUE;
		NSStraight:= TRUE;
		NSGreenLit:= TRUE;
		NSLeft:= FALSE;
		nState:= 7;
	END_IF
	

	7:
	wait(IN:= TRUE, PT:= T#10S);
	IF wait.Q THEN
		wait(In:= FALSE);
		NSGreenLit:= FALSE;
		nState:= 8;
	END_IF
	

	8:
	wait(In:= TRUE, PT:= T#3S);
	wait1(In:= TRUE, PT:= T#0.5S);
	IF wait1.Q THEN
		NSYellowLit:= NOT NSYellowLit;
		wait1(IN:= FALSE);
	END_IF

	IF wait.Q THEN
		wait(IN:= FALSE);
		NSYellowLit:= FALSE;
		NSLeft:= TRUE;
		NSRedLit:= TRUE;
		NSLeftGLit:= TRUE;
		EWStraight:= FALSE;
		nState:= 9;
	END_IF
	

	9:
	wait(In:= TRUE, PT:= T#5S);
	IF wait.Q THEN
		wait(IN:= FALSE);
		NSLeftGLit:= FALSE;
		hideNSLeftGLit:= TRUE;
		nState:= 10;
	END_IF
	

	10:
	wait1(IN:= TRUE, PT:= T#0.5S);
	wait(IN:= TRUE, PT:= T#3S);
	IF wait1.Q THEN
		NSLeftYLit:= NOT NSLeftYLit;
		wait1(IN:= FALSE);
	END_IF

	IF wait.Q THEN
		wait(in:= FALSE);
		NSLeftYLit:= FALSE;
		hideNSLeftYLit:= TRUE;
		NSLeftRLit:= TRUE;
		EWStraight:= TRUE;
		nState:= 0;
	END_IF
END_CASE
```

</details>

## HMI Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=15iRosWs3HIAC7Vd_JZJHBKMR84VWVFkD" alt="fig_2.png">
</p>




***



<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
