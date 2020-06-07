---
layout: post
mathjax: true
title: 'Übung: Lottery Turntable'
description: 'Contributor: Panyang Xiang'
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='übung01_SushiBelt.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='ch3.html';">Next</button></span>
</p>

# Introduction
This program performs the function of a lottery turntable.

# Program Design
## Device Hierarchy

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1hLLW7TvOY8VTFSAgtZ-LR1m-CEKGU40z" alt="fig_1.png">
</p>

## Main Program
<details>
    <summary>PROGRAM PLC_PRG (VAR Declaration)</summary>
    
```
PROGRAM PLC_PRG
VAR
	only1button: BOOL;//提示只能按一个按钮指示灯
	bln: BOOL; //控制回零按钮
	start, stop: BOOL; //开始和停止按钮
	restart: BOOL; //控制重新开始指示灯
	startrtrig, stoprtrig: R_TRIG;//开始与停止上升沿触发
	blnrtrig: R_TRIG;//回零上升沿触发
	blnftrig: F_TRIG;//回零下降沿触发
	nState: BYTE;
	nnState: BYTE;
	fbpower: MC_Power;//使能
	moveabs: MC_MoveAbsolute;
	movevel: MC_MoveVelocity;
	stopmove: MC_Halt;
	wait: TON;
	zeropos: LREAL;//定义的零点位置
	stoppos: LREAL;//准盘停止位置
	consolation: BOOL;//鼓励奖指示灯
	thirdprize: BOOL;//三等奖指示灯
	secondprize: BOOL;//二等奖指示灯
	firstprize: BOOL;//一等奖指示灯
	
END_VAR
```

</details>

<details>
    <summary>PROGRAM PLC_PRG (Main)</summary>
    
```
fbpower(Axis:= AxisX, Enable:= TRUE, bRegulatorOn:= TRUE, bDriveStart:= TRUE);//使能
movevel(Axis:= AxisX, Execute:= FALSE);//复位

//back to zero 回零
CASE nState OF
	0:
	IF fbpower.Status THEN
		nState:= 1;
	END_IF
	
	1: //如果已在零位，到case 2； 如果不在零位到case 3.
	IF bln THEN
		nState:= 2;
	ELSE
		nState:= 3;
	END_IF
	
	2:
	movevel(Axis:= AxisX, Execute:= TRUE, Velocity:= 300, acceleration:= 200, deceleration:= 200,Direction:= MC_Direction.Negative); //已在零位，反向转动
	blnftrig(CLK:= bln); //下降沿触发
	IF blnftrig.Q THEN
		zeropos:= AxisX.fActPosition; //轴刚好离开零点触发下降沿，记录刚好离开零点的瞬时位置
		movevel(Axis:= AxisX, Execute:= FALSE); //复位
		stopmove(Axis:= AxisX, Execute:= FALSE); //复位
		nState:= 4;
	END_IF
	
	3:
	movevel(Axis:= AxisX, Execute:= TRUE, Velocity:= 300, acceleration:= 200, deceleration:= 200,Direction:= MC_Direction.Positive); //不在零位，正向转动
	blnrtrig(CLK:= bln); //上升沿触发
	IF blnrtrig.Q THEN
		zeropos:= AxisX.fActPosition; //轴刚好经过零点，触发上升沿，记录刚好经过零点的瞬时位置
		movevel(Axis:= AxisX, Execute:= FALSE);//复位
		stopmove(Axis:= AxisX, Execute:= FALSE);//复位
		nState:= 4;
	END_IF
	
	4:
	stopmove(Axis:= AxisX, Execute:= TRUE, Deceleration:= 200); //停止轴运动
	IF stopmove.Done THEN
		stopmove(Axis:= AxisX, Execute:= FALSE); //复位
		moveabs(Axis:= AxisX, Execute:= FALSE); //复位
		nState:= 5;
	END_IF
	
	5:
	moveabs(Axis:= AxisX, Execute:= TRUE, Position:= zeropos,Velocity:= 300, Acceleration:= 200, Deceleration:= 200); //轴在刚好经过或刚好离开零点时不会立刻停下，所以使用绝对位置移动到zeropos。
	IF moveabs.Done THEN
		nState:=6;
	END_IF
	
	6:	
	moveabs(Axis:= AxisX, Execute:= FALSE);//复位
END_CASE

//Start the game 开始转盘抽奖

CASE nnState OF
	0:
	IF fbpower.Status THEN
		movevel(Axis:=AxisX,Execute:=FALSE);
		nnState:= 1;
	END_IF
	
	1://如果同时按几个按钮，之后的nnstate不会执行.
	IF NOT (bln AND start) OR NOT (bln AND stop) OR NOT (start AND stop) OR NOT (bln AND start AND stop) THEN
		nnState:= 2;
		only1button:= TRUE;
	END_IF
	
	2:
	IF start THEN
		consolation:= FALSE;//开始以后复为所有指示灯和计时器
		thirdprize:= FALSE;
		secondprize:= FALSE;
		firstprize:= FALSE;
		restart:= FALSE;
		wait(IN:= FALSE, PT:= T#3S); 
		movevel(Axis:= AxisX, Execute:= TRUE, Velocity:= 300, Acceleration:= 200, Direction:= MC_Direction.Positive);
		nnState:=3; 
	END_IF
	
	3:
	IF stop THEN
		stopmove(Axis:=AxisX, Execute:=FALSE); 
		nnState:= 4;
	END_IF
	
	4:
	wait(IN:= TRUE, PT:= T#3S);//触发成功，先计时3s
	IF wait.Q THEN
		movevel(Axis:= AxisX, Execute:= FALSE);//3秒以后，复位轴运动模块
		stopmove(Axis:= AxisX, Execute:= TRUE, Deceleration:= 2000);//停止轴运动
		IF stopmove.Done THEN
			stopmove(Axis:=AxisX, Execute:= FALSE);
			nnState := 5;
		END_IF
	END_IF
	
	5:
		stoppos:= AxisX.fActPosition; //记录轴停止后的位置
		nnState:= 6;

	6:
	IF stoppos <= 360 THEN
		nnState:= 7;
    ELSE
		nnState:= 8;
	END_IF
	
	7:
	stoppos:= stoppos;
	nnState:= 9;
	
	8:
	WHILE stoppos > 360 DO
		stoppos:= stoppos - 360;
	END_WHILE
	nnState:= 9;
	
	9: //设定轴停止的位置分别对应什么奖励
	IF stoppos > 0 AND stoppos < 160 THEN
		consolation:= TRUE;
	
	ELSIF stoppos > 320 AND stoppos < 360 THEN
		firstprize:= TRUE;
	
	ELSIF stoppos > 160 AND stoppos < 260 THEN
		thirdprize:= TRUE;
		
	ELSIF stoppos > 260 AND stoppos < 320 THEN
		secondprize:= TRUE;

	ELSIF stoppos = 0 OR stoppos = 160 OR stoppos = 260 OR stoppos = 320 OR stoppos = 360 THEN //若轴停止的位置在两个区域的分界线上，需要重新开始转盘抽奖
		consolation:= FALSE;
		thirdprize:= FALSE;
		secondprize:= FALSE;
		firstprize:= FALSE;
		restart:= TRUE; 
	END_IF
	nnState:= 0; //抽奖完毕后，可以再次开始抽奖
END_CASE
```

</details>

## HMI Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1X1tjAU5Wary_8r925U7h3IH_HbbY6JFO" alt="fig_2.png">
</p>


***

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
