---
layout: post
mathjax: true
title: 'Übung: Unmanned Parking Station'
description: 'Contributor: Panyang Xiang'
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='übung04_solarSystem.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='übung06_trafficLight.html';">Next</button></span>
</p>

# Program Design
## Device Hierarchy
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=144tPOrLw4vnmEzM3T_-qE1MG6a5rSsn_" alt="fig_1.png">
</p>

## PLC_PRG
<details>
    <summary>VAR Declaration</summary>
    
```
PROGRAM PLC_PRG
VAR
	power: BOOL;//开机与关机
	fbpower1, fbpower2: MC_Power;//两个轴使能命名
	hideinlight, hideoutlight: BOOL;//隐藏入口与出口的屏幕显示内容
	hideseefee: BOOL;//隐藏停车费显示内容
	parkin, parkout: BOOL;//进入停车与离开感应
	moveabs1, moveabs2: MC_MoveAbsolute;//两个轴的绝对运动命名
	moverela1, moverela2: MC_MoveRelative;//两个轴的相对运动命名
	hideclose1: BOOL:= FALSE;//隐藏入口闸口的关闭状态
	hideclose2: BOOL:= FALSE;//隐藏出口闸口的关闭状态
	hideopen1: BOOL:= TRUE;//隐藏入口闸口的开启状态
	hideopen2: BOOL:= TRUE;//隐藏出口闸口的开启状态
	payfee: BOOL;//缴费
	carpass1, carpass2: BOOL;//汽车在入口与出口通过的感应
	fullnum: INT:= 1;//总车位数
	fullnotice: BOOL;//停车已满提示
	counter: CTU;//计数器
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
//初始化使能，MC_moveabsolute与MC_moverelative功能块
moveabs1(Axis:= AxisX, Execute:= FALSE);
moveabs2(Axis:= AxisY, Execute:= FALSE);
moverela1(Axis:= AxisX, Execute:= FALSE);
moverela2(Axis:= AxisY, Execute:= FALSE);
fbpower1(Axis:= AxisX, Enable:= TRUE, bRegulatorOn:= FALSE, bDriveStart:= FALSE);
fbpower2(Axis:= AxisY, Enable:= TRUE, bRegulatorOn:= FALSE, bDriveStart:= FALSE);

//点击开机/关机按钮让轴使能或不使能
IF power THEN
	fbpower1(Axis:= AxisX, Enable:= TRUE, bRegulatorOn:= TRUE, bDriveStart:= TRUE);
	fbpower2(Axis:= AxisY, Enable:= TRUE, bRegulatorOn:= TRUE, bDriveStart:= TRUE);
ELSE
	power:= FALSE;
END_IF

//如果轴使能，进口与出口屏幕亮起；如果没使能，屏幕不亮
IF fbpower1.Status THEN
	hideinlight:= FALSE;
ELSE
	hideinlight:= TRUE;
END_IF

IF fbpower2.status THEN
	hideoutlight:= FALSE;
	hideseefee:= FALSE;
ELSE
	hideoutlight:= TRUE;
	hideseefee:= TRUE;
END_IF

//如果检测到有车停入，轴开使正方向相对运动至90度
IF parkin THEN
	moverela1(Axis:= AxisX, Execute:= TRUE, Distance:= 90, Velocity:= 40, Acceleration:= 10, Deceleration:= 10);
END_IF
IF moverela1.Done THEN//如果运动完毕，闸口开启
	hideclose1:= TRUE;
	hideopen1:= FALSE;
	moverela1(Axis:= AxisX, Execute:= FALSE);//复位相对运动块
END_IF

//如果检测到入口车在闸口开启后已经完全通过，闸口开始关闭
IF carpass1 THEN
	moveabs1(Axis:= AxisX, Execute:= TRUE, Position:= 0, Velocity:= 40, Acceleration:= 10, Deceleration:= 10, Direction:= MC_Direction.Negative);
END_IF
IF moveabs1.Done THEN
	moveabs1(Axis:= AxisX, Execute:= FALSE);
	hideopen1:= TRUE;
	hideclose1:= FALSE;
END_IF
	
	
//如果检测到有车离开，轴开使反方向运动至90度
IF parkout THEN
	IF payfee THEN
		moverela2(Axis:= AxisY, Execute:= TRUE, Distance:= -90, Velocity:= 40, Acceleration:= 10, Deceleration:= 10);
	END_IF
END_IF
IF moverela2.Done THEN//如果运动完毕，闸口开启
	hideclose2:= TRUE;
	hideopen2:= FALSE;
	parkout:= FALSE;
END_IF
		
//如果检测到出口处车辆已经完全离开，闸口开始关闭
IF carpass2 THEN
	moveabs2(Axis:= AxisY, Execute:= TRUE, Position:= 0, Velocity:= 40, Acceleration:= 10, Deceleration:= 10, Direction:= MC_Direction.Positive);
END_IF
IF moveabs2.Done THEN
	moveabs2(Axis:= AxisY, Execute:= FALSE);
	hideopen2:= TRUE;
	hideclose2:= FALSE;
END_IF
```

</details>

## HMI Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1bgIUzOesgOdvlN7-hHNwU3WGxtKTCchz" alt="fig_2.png">
</p>



***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
