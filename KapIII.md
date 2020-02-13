---
layout: post
mathjax: true
title: 'Kapitel III: Brief Introduction to Functional Block'
description: 
is_project_page: false
---

<p style="text-align:center;">
<button type="button" onclick="window.location.href='index.html';">Homepage</button>
<span style="float:left;"><button type="button" onclick="alert('This is the first chapter!')">Previous</button></span>
<span style="float:right;"><button type="button" onclick="window.location.href='ch3.html';">Next</button></span>
</p>

# What is Function Block after all?
This is a function block (FB for short):

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1amHJybsa-439PakmB722y4ABIhgTumHv" alt="fig_1_1.jpg" width="250">
</p>

Consider it as some sort of interface that tells the user to feed in some data, and then returns some other data to the user. FB only specifies what kind of data goes into it, what ought to be achieved using these input data, and what kind of data it should output. N.B. that FB itself does not do anything at all, it serves soely as an **interface**, the job is left for hardware suppliers to implement. For example, the FB called _"MC_Power"_ is used to _"enable the axes"_: 

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=16vzgxgpw30zjiu4_O8BQoKWiwu9ma9ZS" alt="fig_1_2.jpg" width="250">
</p>

By _"enable"_ it simply means to power up the motor, so the rotor is no longer free to rotate. _MC_Power_ will take in some data, tells the actual implementation to do the powering stuff, and then output some other data. _MC_Power_ is one of the most commonly used FB, and we will take a much detailed look at it later.

## Two Types of FB: Enable and Execute
There are two types of function blocks, namely the "Enable" type and the "Execute" type. Generally, these two types of FB have the following interface:

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1nkDNWHRuA9dm7_61bAqOTLhZwY213XyZ" alt="fig_1_3.jpg">
</p>

The Enable type FB on the left will take in:
1. "Enable". A boolean value (BOOL for short). It could be an input from a push button.

The Enable type FB on the left will yield:
1. "Valid". Also a boolean value. This output tells whether the task is accomplished or not;
2. "Error". BOOL. Tells whether an error had occurred or not;
3. "ErrorID". The datatype _UINT_ means 32-bit unsigned integer, which stores the error identification code that can be used to find a more comprehensive description of the error.

The Execute type FB on the right will take in:
1. "Execute". BOOL. Could be coming from a switch button.

The Execute type FB on the right will yield:
1. "Done". BOOL. Tells whether the task is completed or not;
2. "Busy". BOOL. Tells whether the task is still in execution or not;
3. "Error". BOOL. Tells whether an error had occurred or not;
4. "ErrorID". UINT. Stores an error identification code.

***

### Closer Look at Enable-FBs
Most of the Enable-FBs has the following interface:

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1EaQM9fQVW1O0a-6R6Df5jPipTyRb7E56" alt="fig_1_4.jpg" width="550">
</p>

The variables squared by the orange line are the typical variables in such FBs. In most motion control related FBs, _Var1_ would be "Axis" accepting input datatype "AXIS_REF". We will touch on this later. The misterious horizontal line inside the FB will also be explained in section **More on MC_Power**.
In order to describe the behaviour of Enable-type FB, a tool called **Sequence Diagram** would be conducive here. The seqeunce diagram adopted here is made self-explanatory by the following sketch:

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1KZc53LnkzE0GEWj602GyqWpQciNu7D9R" alt="fig_1_5.jpg" width="550">
</p>

With the help of sequence diagram, the common behaviour of Enable-type FB can be illustrated using the following cases, as shown below.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=11vMMRoxFYUgkcABPMddzmqXKh-BCQ0gf" alt="fig_1_6.jpg" width="650">
</p>

* Case 1
  * At $$t_{1}$$, the button is pushed and _Enable_ is switched to TRUE. Immediately, _Busy_ is also turned on. Notice that _Valid_ is not turned on simultaneously as it may requires time to complete the task.
  * At $$t_{3}$$, the button is released and _Enable_ is switched off. _Valid_ will be turned off immediately, but _Busy_ will stay on a little bit. There is a time lag because it takes time for the program to store data.
* Case 2
  * At $$t_{4}$$, the button is pushed and _Enable_ is switched on. 
  * At $$t_{6}$$, shit happens and _Error_ is turned on. Notice that _Error_ forces _Valid_ to switch off. If the error could disappear by itself, then _Busy_ will be forced off. In this case, the _Enable_ should be turned off to reset the FB back to normal.
  * At $$t_{7}$$, the button is released and _Enable_ is turned off. Instantly, _Error_ is turned off as well.
* Case 3
  * At $$t_{8}$$, the button is turned on.
  * At $$t_{9}$$, another type of error occurs. This time, the error will not go away automatically, and therefore _Busy_ stays on to solve it. 
  * At sometime between $$t_{10}$$ and $$t_{11}$$, the error is resolved and _Valid_ will be turned on after some time lag.

***

### Closer look at Execute-FBs
Most of the Execute-FBs has the following interface:

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1oQ_VFRa6VtDx6cVESXIIPsVZDVMuROE9" alt="fig_1_7.jpg" width="550">
</p>

On the RHS, there are two new output variables, namely the _Done/InXXX_, the _CommandAborted_, and the _Active_:
* _InXXX_: It could be _InVelocity_, _InGear_, _InSync_, etc. Taking _InVelocity_ as an example, it will be turned on when the axis has reached a specified velocity given sometime to accelerate. In general, for motion control that involves acceleration, deceleration, or synchronization, the corresponding FB will have _InXXX_ in place of _Done_.
* _CommandAborted_: It will be turned on when the execution of the current FB is interrupted by other FB.
* _Active_: It will be turned on when the axis is carrying out the task.

Since there are two sub-types of the Execute-FB, viz. the Execute/Done sub-type and the Execute/InXXX sub-type, their behaviour defers.

***

#### Execute/Done Type

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=12aEX_9c4g4WYG6pOMUvKrctl0Wu-j09V" alt="fig_1_8.jpg" width="650">
</p>

As shown above, the typical behaviour of Execute/Done type FB can be splitted into 3 cases:
* Case 1:
  * At $$t_{1}$$, a button is pushed so _Execute_ turns on. Immediately, _Busy_ turns on to show that the FB is working on it. After some lag, _Active_ turns on to display that the axis is adjusting itself towards end state.
  * At $$t_{2}$$, the action is suspended by commands from other FBs, therefore _CommandAborted_ turns on. In reaction to that, _Busy_ is forced off.
  * At $$t_{3}$$, the push button is released so the FB is reset.
* Case 2:
  * At $$t_{5}$$, error occurred so _Busy_ is forced off.
* Case 3:
  * Normal operation.
  
***
  
#### Execute/InXXX Type

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=145TQ-pQBXx-ywnS7N88WNwx0b-aD_LGw" alt="fig_1_9.jpg" width="650">
</p>

As shown above, the typical behaviour of Execute/InXXX type FB can also be splitted into 3 cases. Since the above sketch is quite straightforward, the description is skipped here.

***

## More on MC_Power
The following figure shows the actuall interface of the function block _"MC_Power"_. The graphical representation used in the figure is actually a programming language called _**Sequential Function Chart(SFC)**_. SFC is often employed in big projects so as to give a lucid overview of the program structure.

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1zAG3frIaMsM432Xlgs4vkxwN69DyTbwI" alt="fig_1_10.jpg" width="450">
</p>

As you can see, the interface is much the same to the general interface of Enable-type FB. Tip: most of the IDEs are equipped with help manual which allows you to consult the definition of each variables. Here is a screenshot of the description of the _MC_Power_ FB:

<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=1E_E1oCWJvMrX3K04Add8osXuoT4YnwRH" alt="fig_1_11.jpg">
</p>

The horizontal line inside the FB means that the FB will not store the data locally but instead reference it. This is because the datatype _AXIS_REF_ may be too large to be stored locally (it might just contain >100 variables!) in the FB. Also, _AXIS_REF_ may be different for different hardware. For instance, motor manufacturerd by supplier A might have the following _AXIS_REF_:
```
TYPE
    AXIS_REF : STRUCT
        AXIS_NO             : UINT;
        AXIS_TYPE           : WORD;
        AXIS_NAME           : STRING(255);
        AXIS_MAX_SPEED      : UINT;
        ....
    END_STRUCT;
END_TYPE
```
And that of supplier B has:
```
TYPE
    AXIS_REF : STRUCT
        AXIS_NAME           : DWORD;
        AXIS_CODE           : LONG;
        MAX_WARP_SPEED      : ULINT;
        ....
    END_STRUCT;
END_TYPE
```

This function block can also be expressed in _**Structured Text(ST)**_, which is also a frequently used programming language conformed to IEC61131-3.
```
PROGRAM PLC_PRG
VAR
    fbPower     :   MC_Power;   
    // similar to 'import numpy as np' in python
END_VAR
```
```
fbPower(Axis := myWarpDrive_1, Enable:= TRUE, bDriveStart := TRUE, bRegulatorOn := TRUE);   
// Enable Axis
```


<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
