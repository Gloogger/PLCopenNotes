---
layout: post
mathjax: true
title: 'Übung: Vending Machine'
description: 'Contributor: Panyang Xiang'
is_project_page: false
---


<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="window.location.href='übung02_lotteryTurntable.html';">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='übung04_solarSystem.html';">Next</button></span>
</p>

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1Y6z9axoPeuhnUAVT8uCEIz3k28ulIhwv" alt="fig_1.png">
</p>

# Introduction
This program simulates a small vending machine.

# Program Design
## Device Hierarchy

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1-aI7FqO8wF0j7OkxYxq_V7YhXdtaH2br" alt="fig_2.png">
</p>

## Main Program

<details>
    <summary>PROGRAM PLC_PRG (VAR Declaration)</summary>
    
```
PROGRAM PLC_PRG
VAR
	nState: UDINT;
	drink1, drink2, drink3, drink4, drink5, drink6: BOOL;
	hidedrink1, hidedrink2, hidedrink3, hidedrink4, hidedrink5, hidedrink6: BOOL;
	power: BOOL;
	button1, button2, button3, button4, button5, button6: BOOL;
	confirm: BOOL;
	hideshow: BOOL:= TRUE;
	x: INT:= 0;
	y: INT:= 0;
	xplus, yplus: INT:= 1;
	xstop, ystop: INT:= 0;
	wait1, wait2, wait3, wait4, wait5, wait6: TON;
	hideout: BOOL:= TRUE;
	xmove: BOOL;
	ymove: BOOL;
	shownum: REAL;
	price: REAL;
	pay: BOOL;
	afterpay: STRING;
	cancel: BOOL;
END_VAR
```

</details>

<details>
    <summary>PROGRAM PLC_PRG (Main)</summary>
    
```
IF power THEN
	hideshow:= FALSE;
ELSE
	hideshow:= TRUE;
END_IF


CASE nState OF
	0:
	IF power THEN
		nState:= 10;
	ELSE
		nState:= 0;
	END_IF
	
	10:
	IF button1 AND confirm THEN
		shownum:= 1;
		price:= 4.5;
		drink1:= TRUE;
		nState:= 1;
	ELSIF button2 AND confirm THEN
		shownum:= 2;
		price:= 3.5;
		drink2:= TRUE;
		nState:= 2;
	ELSIF button3 AND confirm THEN
		shownum:= 3;
		price:= 5.5;
		drink3:= TRUE;
		nState:= 3;
	ELSIF button4 AND confirm THEN
		shownum:= 4;
		price:= 3;
		drink4:= TRUE;
		nState:= 4;
	ELSIF button5 AND confirm THEN
		shownum:= 5;
		price:= 2;
		drink5:= TRUE;
		nState:= 5;
	ELSIF button6 AND confirm THEN
		shownum:= 6;
		price:= 6;
		drink6:= TRUE;
		nState:= 6;
	END_IF
	
	1:
	IF cancel THEN
		button1:= FALSE;
		shownum:= 0;
		price:= 0;
		drink1:= FALSE;
		nState:= 0;
	END_IF
	IF pay THEN
		xmove:= TRUE;
		afterpay:= 'Thank you!';
	END_IF
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -200 THEN
		xmove:= FALSE;
		ymove:= TRUE;
	END_IF
	
	IF ymove THEN
		y:= y - yplus;
	END_IF
	
	IF y = -300 THEN
		ymove:= FALSE;
		nState:= 11;
	END_IF
	
	
	11:
	wait1(IN:= TRUE, PT:= T#2S);
	IF wait1.Q THEN
		hidedrink1:= TRUE;
		wait1(IN:= FALSE);
		ymove:= TRUE;
		nState:= 111;
	END_IF

	111:
	IF ymove THEN
		y:= y + yplus;
	END_IF
	
	IF y = 0 THEN
		ymove:= FALSE;
		wait1(IN:= TRUE, PT:= T#2S);
		IF wait1.Q THEN
			hideout:= FALSE;
			xmove:= TRUE;
		END_IF
	END_IF
	
	IF xmove THEN
		x:= x + xplus;
	END_IF
	
	IF x = 0 THEN
		wait1(IN:= FALSE);
		xmove:= FALSE;
		nState:= 1111;
	END_IF
	
	1111:
	wait1(IN:= TRUE, PT:= T#2S);
	IF wait1.Q THEN
		hideout:= TRUE;
		button1:= FALSE;
		afterpay:= '';
		price:= 0;
		shownum:= 0;
		hidedrink1:= FALSE;
		drink1:= FALSE;
		nState:= 0;
		wait1(IN:= FALSE);
	END_IF
	
	2:
	IF cancel THEN
		button2:= FALSE;
		shownum:= 0;
		price:= 0;
		drink2:= FALSE;
		nState:= 0;
	END_IF
	IF pay THEN
		xmove:= TRUE;
		afterpay:= 'Thank you!';
	END_IF
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -100 THEN
		xmove:= FALSE;
		ymove:= TRUE;
	END_IF
	
	IF ymove THEN
		y:= y - yplus;
	END_IF
	
	IF y = -300 THEN
		ymove:= FALSE;
		nState:= 22;
	END_IF
	
	22:
	wait2(IN:= TRUE, PT:= T#2S);
	IF wait2.Q THEN
		hidedrink2:= TRUE;
		wait2(IN:= FALSE);
		ymove:= TRUE;
		nState:= 222;
	END_IF
	
	222:
	IF ymove THEN
		y:= y + yplus;
	END_IF
	
	IF y = 0 THEN
		ymove:= FALSE;
		xmove:= TRUE;
	END_IF
	
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -200 THEN
		xmove:= FALSE;
		nState:= 2222;
	END_IF
	
	2222:
	wait2(IN:= TRUE, PT:= T#2S);
	IF wait2.Q THEN
		hideout:= FALSE;
		xmove:= TRUE;
		nState:= 22222;
	END_IF
	
	22222:
	IF xmove THEN
		x:= x + xplus;
	END_IF
	IF x = 0 THEN
		wait2(IN:= FALSE);
		xmove:= FALSE;
		nState:= 222222;
	END_IF
	
	222222:
	wait2(IN:= TRUE, PT:= T#2S);
	IF wait2.Q THEN
		hideout:= TRUE;
		button2:= FALSE;
		afterpay:= '';
		price:= 0;
		shownum:= 0;
		wait2(IN:= FALSE);
		drink2:= FALSE;
		hidedrink2:= FALSE;
		nState:= 0;
	END_IF
	
	3:
	IF cancel THEN
		button3:= FALSE;
		shownum:= 0;
		price:= 0;
		drink3:= FALSE;
		nState:= 0;
	END_IF
	IF pay THEN
		ymove:= TRUE;
		afterpay:= 'Thank you!';
	END_IF
	IF ymove THEN
		y:= y - yplus;
	END_IF
	
	IF y = -300 THEN
		ymove:= FALSE;
		nState:= 33;
	END_IF
	
	33:
	wait3(IN:= TRUE, PT:= T#2S);
	IF wait3.Q THEN
		hidedrink3:= TRUE;
		wait3(IN:= FALSE);
		ymove:= TRUE;
		nState:= 333;
	END_IF
	
	333:
	IF ymove THEN
		y:= y + yplus;
	END_IF
	
	IF y = 0 THEN
		ymove:= FALSE;
		xmove:= TRUE;
	END_IF
	
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -200 THEN
		xmove:= FALSE;
		nState:= 3333;
	END_IF
	
	3333:
	wait3(IN:= TRUE, PT:= T#2S);
	IF wait3.Q THEN
		hideout:= FALSE;
		xmove:= TRUE;
		nState:= 33333;
	END_IF
	
	33333:
	IF xmove THEN
		x:= x + xplus;
	END_IF
	IF x = 0 THEN
		wait3(IN:= FALSE);
		xmove:= FALSE;
		nState:= 333333;
	END_IF
	
	333333:
	wait3(IN:= TRUE, PT:= T#2S);
	IF wait3.Q THEN
		hideout:= TRUE;
		button3:= FALSE;
		afterpay:= '';
		price:= 0;
		shownum:= 0;
		wait3(IN:= FALSE);
		drink3:= FALSE;
		hidedrink3:= FALSE;
		nState:= 0;
	END_IF
	
	4:
	IF cancel THEN
		button4:= FALSE;
		shownum:= 0;
		price:= 0;
		drink4:= FALSE;
		nState:= 0;
	END_IF
	IF pay THEN
		xmove:= TRUE;
		afterpay:= 'Thank you!';
	END_IF
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -200 THEN
		xmove:= FALSE;
		ymove:= TRUE;
	END_IF
	
	IF ymove THEN
		y:= y - yplus;
	END_IF
	
	IF y = -140 THEN
		ymove:= FALSE;
		nState:= 44;
	END_IF
	
	44:
	wait4(IN:= TRUE, PT:= T#2S);
	IF wait4.Q THEN
		hidedrink4:= TRUE;
		wait4(IN:= FALSE);
		ymove:= TRUE;
		nState:= 444;
	END_IF
	
	444:
	IF ymove THEN
		y:= y + yplus;
	END_IF
	
	IF y = 0 THEN
		ymove:= FALSE;
		wait4(IN:= TRUE, PT:= T#2S);
		IF wait4.Q THEN
			hideout:= FALSE;
			xmove:= TRUE;
		END_IF
	END_IF
	
	IF xmove THEN
		x:= x + xplus;
	END_IF
	
	IF x = 0 THEN
		wait4(IN:= FALSE);
		xmove:= FALSE;
		nState:= 4444;
	END_IF
	
	4444:
	wait4(IN:= TRUE, PT:= T#2S);
	IF wait4.Q THEN
		hideout:= TRUE;
		button4:= FALSE;
		afterpay:= '';
		price:= 0;
		shownum:= 0;
		hidedrink4:= FALSE;
		drink4:= FALSE;
		nState:= 0;
		wait4(IN:= FALSE);
	END_IF
	
	5:
	IF cancel THEN
		button5:= FALSE;
		shownum:= 0;
		price:= 0;
		drink5:= FALSE;
		nState:= 0;
	END_IF
	IF pay THEN
		xmove:= TRUE;
		afterpay:= 'Thank you!';
	END_IF
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -100 THEN
		xmove:= FALSE;
		ymove:= TRUE;
	END_IF
	
	IF ymove THEN
		y:= y - yplus;
	END_IF
	
	IF y = -140 THEN
		ymove:= FALSE;
		nState:= 55;
	END_IF
	
	55:
	wait5(IN:= TRUE, PT:= T#2S);
	IF wait5.Q THEN
		hidedrink5:= TRUE;
		wait5(IN:= FALSE);
		ymove:= TRUE;
		nState:= 555;
	END_IF
	
	555:
	IF ymove THEN
		y:= y + yplus;
	END_IF
	
	IF y = 0 THEN
		ymove:= FALSE;
		xmove:= TRUE;
	END_IF
	
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -200 THEN
		xmove:= FALSE;
		nState:= 5555;
	END_IF
	
	5555:
	wait5(IN:= TRUE, PT:= T#2S);
	IF wait5.Q THEN
		hideout:= FALSE;
		xmove:= TRUE;
		nState:= 55555;
	END_IF
	
	55555:
	IF xmove THEN
		x:= x + xplus;
	END_IF
	IF x = 0 THEN
		wait5(IN:= FALSE);
		xmove:= FALSE;
		nState:= 555555;
	END_IF
	
	555555:
	wait5(IN:= TRUE, PT:= T#2S);
	IF wait5.Q THEN
		hideout:= TRUE;
		button5:= FALSE;
		afterpay:= '';
		price:= 0;
		shownum:= 0;
		wait5(IN:= FALSE);
		drink5:= FALSE;
		hidedrink5:= FALSE;
		nState:= 0;
	END_IF
	
	6:
	IF cancel THEN
		button6:= FALSE;
		shownum:= 0;
		price:= 0;
		drink6:= FALSE;
		nState:= 0;
	END_IF
	IF pay THEN
		ymove:= TRUE;
		afterpay:= 'Thank you!';
	END_IF
	IF ymove THEN
		y:= y - yplus;
	END_IF
	
	IF y = -140 THEN
		ymove:= FALSE;
		nState:= 66;
	END_IF
	
	66:
	wait6(IN:= TRUE, PT:= T#2S);
	IF wait6.Q THEN
		hidedrink6:= TRUE;
		wait6(IN:= FALSE);
		ymove:= TRUE;
		nState:= 666;
	END_IF
	
	666:
	IF ymove THEN
		y:= y + yplus;
	END_IF
	
	IF y = 0 THEN
		ymove:= FALSE;
		xmove:= TRUE;
	END_IF
	
	IF xmove THEN
		x:= x - xplus;
	END_IF
	
	IF x = -200 THEN
		xmove:= FALSE;
		nState:= 6666;
	END_IF
	
	6666:
	wait6(IN:= TRUE, PT:= T#2S);
	IF wait6.Q THEN
		hideout:= FALSE;
		xmove:= TRUE;
		nState:= 66666;
	END_IF
	
	66666:
	IF xmove THEN
		x:= x + xplus;
	END_IF
	IF x = 0 THEN
		wait6(IN:= FALSE);
		xmove:= FALSE;
		nState:= 666666;
	END_IF
	
	666666:
	wait6(IN:= TRUE, PT:= T#2S);
	IF wait6.Q THEN
		hideout:= TRUE;
		button6:= FALSE;
		afterpay:= '';
		price:= 0;
		shownum:= 0;
		wait6(IN:= FALSE);
		drink6:= FALSE;
		hidedrink6:= FALSE;
		nState:= 0;
	END_IF
END_CASE
```

</details>


## HMI Design
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1nNvvvaIOShHJdQv6ssIHpzsrImKWynSe" alt="fig_3.png">
</p>


***


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
