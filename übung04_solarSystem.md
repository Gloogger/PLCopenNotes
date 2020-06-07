---
layout: post
mathjax: true
title: 'Übung: Solar System'
description: 'Contributor: Panyang Xiang'
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='übung03_vendingMachine.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='ch3.html';">Next</button></span>
</p>

# Program Design
## Device Hierarchy
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1k_uDTcZqgyBrmyoKAJYiPRXJIrS2befC" alt="fig_1.png">
</p>

## Planet Information STRUCT

Information of each planets of the solar system will be stored as an object in the following format:

```
TYPE PlanetInfo :
STRUCT
    PlanetM : REAL := (mass of the planet in kg);
    PlanetLR : REAL := (radius of the planet in m);
    PlanetAvgV : REAL := (velocity in m/s);
    PlanetT : REAL := (period in s);
END_STRUCT
END_TYPE
```


<details>
    <summary>EarthInfo</summary>
    
```
TYPE EarthInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	EarthM: REAL:= 5.972E24;
	EarthLR: REAL:= 1.496E11;
	EarthSR: REAL:= 1.4958E11;
	EarthAvgV: REAL:= 2.979E4;;
	EarthT: REAL:= 3.154E7;
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>JupitorInfo</summary>
    
```
TYPE JupiterInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	JupiterM: REAL:= 1.898E27;
	JupiterLR: REAL:= 7.785E11;
	JupiterAvgV: REAL:= 1.307E4;
	JupiterT: REAL:= 3.741E8;
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>MarsInfo</summary>
    
```
TYPE MarsInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	MarsM: REAL:= 6.42E23;
	MarsLR: REAL:= 2.279E11;
	MarsAvgV: REAL:= 2.413E4;
	MarsT: REAL:= 5.936E7;
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>MercuryInfo</summary>
    
```
TYPE MercuryInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	MercuryM: REAL:= 3.3022E23;
	MercuryLR: REAL:= 5.791E10;
	MercuryAvgV: REAL:= 4.787E4;
	MercuryT: REAL:= 7.577E6;
	
	
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>NeptuneInfo</summary>
    
```
TYPE NeptuneInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	NeptuneM: REAL:= 1.024E26;
	NeptuneLR: REAL:= 4.503E12;
	NeptuneAvgV: REAL:= 5.43E3;
	NeptuneT: REAL:= 5.212E9;
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>StaurnInfo</summary>
    
```
TYPE SaturnInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	SaturnM: REAL:= 5.685E26;
	SaturnLR: REAL:= 1.433E12;
	SaturnAvgV: REAL:= 9.69E3;
	SaturnT: REAL:= 9.359E8;
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>UranusInfo</summary>
    
```
TYPE UranusInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	UranusM: REAL:= 8.681E25;
	UranusLR: REAL:= 2.877E12;
	UranusAvgV: REAL:= 6.81E3;
	UranusT: REAL:= 2.661E9;
END_STRUCT
END_TYPE
```

</details>


<details>
    <summary>VenusInfo</summary>
    
```
TYPE VenusInfo :
STRUCT
	//units for mass is kg
	//units for velocity is m/s
	//units for radius is m
	//units for period is s
	VenusM: REAL:= 4.8676E24;
	VenusLR: REAL:= 1.082E11;
	VenusAvgV: REAL:= 3.502E4;
	VenusT: REAL:= 1.941E7;
END_STRUCT
END_TYPE
```

</details>

## Global Variables

```
VAR_GLOBAL
	//units for mass is kg
	G: REAL:= 6.67428E-11;
	M: REAL:= 1.9891E30;
	pi: REAL:= 3.141592;
END_VAR
```

## Functions

### CalcRadius
<details>
    <summary>VAR Declaration</summary>
    
```
FUNCTION CalcRadius : REAL
VAR_INPUT
	xPosition: REAL;
	yPosition: REAL;
	SunXPos: REAL;
	SunYPos: REAL;
END_VAR
VAR
	Distance: REAL;
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
//此函数用来计算行星与太阳之间的实时距离（r), r可被带入椭圆运行速度的函数用来计算实时线速度。
//r在这里被表示为Distance
Distance:= SQRT((xPosition-SunXPos)*(xPosition-SunXPos) + (yPosition-SunYPos)*(yPosition-SunYPos))*expt(10,9);
CalcRadius:= Distance;
```

</details>

### CalcS
<details>
    <summary>VAR Declaration</summary>
    
```
FUNCTION CalcS : real
VAR_INPUT
	n: INT;
	UpperLimit: REAL;
	LowerLimit: REAL;
	cst: REAL;
	
END_VAR
VAR
	value: REAl;
	value1: REAL:= 0;
	func: REAL;
	t: REAL;
	i: INT;
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
//func:= cst * t;
FOR i:= 1 TO n DO
	//value:= cst * (LowerLimit + (i - 0.5)*((UpperLimit - LowerLimit)/n));
	value:= cst;
	value1:= value1+((UpperLimit-LowerLimit)/n)*value;

END_FOR
CalcS:= Value1;
```

</details>

### CalcSR
<details>
    <summary>VAR Declaration</summary>
    
```
FUNCTION CalcSR : REAL
VAR_INPUT
	LR: REAL;//半长轴：long radius
	AvgV: REAL;//平均线速度
	T: REAL;//时间
END_VAR
VAR
	SR: REAL;
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
//计算行星的半短轴（SR）：short radius
SR:= (AvgV*T-4*LR)/(2*pi-4);
CalcSR:= SR;
```

</details>

### CalcTheta
<details>
    <summary>VAR Declaration</summary>
    
```
FUNCTION CalcVTheta : REAL
VAR_INPUT
	radius: REAL;//行星与太阳之间的距离
	LR: REAL;
	mass: REAL;//input为太阳质量
END_VAR
VAR
	ThetaDot: REAL;
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
//此函数计算实时角速度
ThetaDot:= SQRT(mass*G*(2/radius-1/LR))/radius;
CalcVTheta:= ThetaDot;
```

</details>

## PlanetSR(PRG)
<details>
    <summary>VAR Declaration</summary>
    
```
PROGRAM PlanetSR
VAR
	MercurySR: REAL;
	VenusSR: REAL;
	EarthSR: REAL;
	MarsSR: REAL;
	JupiterSR: REAL;
	SaturnSR: REAL;
	UranusSR: REAL;
	NeptuneSR: REAL;
	
	MI: MercuryInfo;
	VI: VenusInfo;
	EI: EarthInfo;
	MarI: MarsInfo;
	JI: JupiterInfo;
	SI: SaturnInfo;
	UI: UranusInfo;
	NI: NeptuneInfo;
END_VAR
```

</details>

<details>
    <summary>Main</summary>
    
```
//此函数计算行星的半短轴
MercurySR:= CalcSR(MI.MercuryLR, MI.MercuryAvgV, MI.MercuryT);
VenusSR:= CalcSR(VI.VenusLR, VI.VenusAvgV, VI.VenusT);
EarthSR:= CalcSR(EI.EarthLR, EI.EarthAvgV, EI.EarthT);
MarsSR:= CalcSR(MarI.MarsLR, MarI.MarsAvgV, MarI.MarsT);
JupiterSR:= CalcSR(JI.JupiterLR, JI.JupiterAvgV, JI.JupiterT);
SaturnSR:= CalcSR(SI.SaturnLR, SI.SaturnAvgV, SI.SaturnT); 
UranusSR:= CalcSR(UI.UranusLR,UI.UranusAvgV, UI.UranusT);
NeptuneSR:= CalcSR(NI.NeptuneLR, NI.NeptuneAvgV, NI.NeptuneT);
```

</details>

## PLC_PRG(PRG)
<details>
    <summary>PROGRAM PLC_PRG (VAR Announcement)</summary>
    
```
PROGRAM PLC_PRG
VAR
	cst: REAL;
	n: INT:= 1000;
	EarthL: REAL;
	EarthS: REAL;
	t: TIME;//时间初始值为0
	DeltaT: REAL:= 1E4;//时间变化（设为1E5的原因为放大行星的移动，让肉眼可见）。DeltaT不能设过于大，因为会增大误差使行星偏离轨道.
	Timetotal: REAL;
	nState: BYTE;
	start: BOOL;//开始/暂停按钮
	reset: BOOL;//重置按钮
	wait: TON;//计时器，记录运行时间
	StartXPos, StartYPos: INT:= 0;//所有行星的初始位置均为0
	
	ThetaOld1, ThetaOld2, ThetaOld3, ThetaOld4, ThetaOld5, ThetaOld6, ThetaOld7, ThetaOld8: REAL:= 0;//用来储存Theta的变化
	MercuryX, MercuryY: REAL;//水星X坐标与水星Y坐标
	VenusX, VenusY: REAL;
	EarthX, EarthY: REAL;
	MarsX, MarsY: REAL;
	JupiterX, JupiterY: REAL;
	SaturnX, SaturnY: REAL;
	NeptuneX, NeptuneY: REAL;
	UranusX, UranusY: REAL;
	
	MI: MercuryInfo;//调用水星资料，以下同理
	VI: VenusInfo;
	EI: EarthInfo;
	MarI: MarsInfo;
	JI: JupiterInfo;
	SI: SaturnInfo;
	UI: UranusInfo;
	NI: NeptuneInfo;
END_VAR
```

</details>
<details>
    <summary>PROGRAM PLC_PRG (VAR Announcement)</summary>
    
```
//计算星球的短半轴长度
PlanetSR();
wait(In:= start, pt:= T#1000000S);//计时器，记录点击start按钮之后，行星运行的真实时间
//如果点击start，会执行以下操作
//注意，start按钮为toggle形式，再次点击，会暂停运行。
IF start THEN
	CASE nState OF
		0://计算每颗行星的实时角速度并带入椭圆方程中, 求得行星的实时位置
		IF wait.IN THEN
			t:= wait.ET;
		END_IF
		MercuryX:= (-MI.MercuryLR + MI.MercuryLR*COS(ThetaOld1 + CalcVTheta(CalcRadius(MercuryX, MercuryY,-47,0), MI.MercuryLR, M)*DeltaT))/EXPT(10,9);
		MercuryX:= LREAL_TO_INT(MercuryX);//转换实数到整数（因为absolute movement只接受整数）
		MercuryY:= PlanetSR.MercurySR*SIN(ThetaOld1 + CalcVTheta(CalcRadius(MercuryX, MercuryY,-47,0), MI.MercuryLR, M)*DeltaT)/EXPT(10,9);
		MercuryY:= LREAL_TO_INT(MercuryY);
		
		VenusX:= (-VI.VenusLR + VI.VenusLR*COS(ThetaOld2 + CalcVTheta(CalcRadius(VenusX, VenusY,-108,0), VI.VenusLR, M)*DeltaT))/EXPT(10,9);
		VenusX:= LREAL_TO_INT(VenusX);
		VenusY:= PlanetSR.VenusSR*SIN(ThetaOld2 + CalcVTheta(CalcRadius(VenusX, VenusY,-108,0), VI.VenusLR, M)*DeltaT)/EXPT(10,9);
		VenusY:= LREAL_TO_INT(VenusY);
		
		EarthX:= (-EI.EarthLR + EI.EarthLR*COS(ThetaOld3 + CalcVTheta(CalcRadius(EarthX, EarthY,-167,0), EI.EarthLR, M)*DeltaT))/EXPT(10,9);
		EarthX:= LREAL_TO_INT(EarthX);
		EarthY:= PlanetSR.EarthSR*SIN(ThetaOld3 + CalcVTheta(CalcRadius(EarthX, EarthY,-167,0), EI.EarthLR, M)*DeltaT)/EXPT(10,9);
		EarthY:= LREAL_TO_INT(EarthY);
		EarthL:= EI.EarthM*EXPT(CalcRadius(EarthX, EarthY,-167,0),2) * CalcVTheta(CalcRadius(EarthX, EarthY,-167,0), EI.EarthLR, M)/EXPT(10,38);
		EarthS:= CalcS(n,timetotal,0, EarthL/(2*EI.EarthM))*1E18;
		cst:= EarthL/(2*EI.EarthM)*1E23;
		
		
		MarsX:= (-MarI.MarsLR + MarI.MarsLR*COS(ThetaOld4 + CalcVTheta(CalcRadius(MarsX, MarsY,-228,0), MarI.MarsLR, M)*DeltaT))/EXPT(10,9);
		MarsX:= LREAL_TO_INT(MarsX);
		MarsY:= PlanetSR.MarsSR*SIN(ThetaOld4 + CalcVTheta(CalcRadius(MarsX, MarsY,-228,0), MarI.MarsLR, M)*DeltaT)/EXPT(10,9);
		MarsY:= LREAL_TO_INT(MarsY);
		
		JupiterX:= (-JI.JupiterLR + ji.JupiterLR*COS(ThetaOld5 + CalcVTheta(CalcRadius(JupiterX, JupiterY,-738,0), JI.JupiterLR, M)*DeltaT))/EXPT(10,9);
		JupiterX:= LREAL_TO_INT(JupiterX);
		JupiterY:= PlanetSR.JupiterSR*SIN(ThetaOld5 + CalcVTheta(CalcRadius(JupiterX, JupiterY,-738,0), JI.JupiterLR, M)*DeltaT)/EXPT(10,9);
		JupiterY:= LREAL_TO_INT(JupiterY);
		
		//每计算一次行星的实时位置，对行星已运行过的角度进行累计，并带入到下一次循环中（用以求得新的行星位置）
		//式中的所有常数为太阳相对于各行星轨道零点的坐标
		ThetaOld1:= ThetaOld1 + CalcVTheta(CalcRadius(MercuryX, MercuryY,-47,0), MI.MercuryLR, M)*DeltaT;
		ThetaOld2:= ThetaOld2 + CalcVTheta(CalcRadius(VenusX, VenusY,-108,0), VI.VenusLR, M)*DeltaT;
		ThetaOld3:= ThetaOld3 + CalcVTheta(CalcRadius(EarthX, EarthY,-167,0), EI.EarthLR, M)*DeltaT;
		ThetaOld4:= ThetaOld4 + CalcVTheta(CalcRadius(MarsX, MarsY,-228,0), MarI.MarsLR, M)*DeltaT;
		ThetaOld5:= ThetaOld5 + CalcVTheta(CalcRadius(JupiterX, JupiterY,-738,0), JI.JupiterLR, M)*DeltaT;
		Timetotal:= timetotal+deltaT;
		nState:= 0;

		(*1://计算
		wait(In:= TRUE, pt:= T#1S);
		IF wait.Q THEN
			wait(In:= FALSE);
			t:= t + 1;
			nState:= 0;
		END_IF*)
		
	END_CASE
END_IF

//如果点击reset，所有操作复位
//reset为tap button
IF reset THEN
	MercuryX:= StartXPos;
	MercuryY:= StartYPos;
	
	VenusX:= StartXPos;
	VenusY:= StartYPos;
	
	EarthX:= StartXPos;
	EarthY:= StartYPos;
	EarthL:= 0;
	EarthS:= 0;
	timetotal:= 0;
	
	MarsX:= StartXPos;
	MarsY:= StartYPos;
	
	JupiterX:= StartXPos;
	JupiterY:= StartYPos;
	
	ThetaOld1:= 0;
	ThetaOld2:= 0;
	ThetaOld3:= 0;
	ThetaOld4:= 0;
	ThetaOld5:= 0;
	start:= FALSE;
END_IF
```

</details>



## HMI Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1h49IhFYpkVLMX-CRbv4qQuFzy2XOl77s" alt="fig_2.png">
</p>



***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
