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

## HMI Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1h49IhFYpkVLMX-CRbv4qQuFzy2XOl77s" alt="fig_2.png">
</p>



***

<details>
    <summary>PROGRAM PLC_PRG (VAR Announcement)</summary>
    
```
aaaa
```

</details>

***

Image hosting template:

```
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=" alt="Erklaerung">
</p>
```


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
