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

## What is function block after all?
This is a function block (FB for short):
<p align="center">
    <img src="https://lh3.googleusercontent.com/ngotE6zvjkKjTvIp-zRD9Cxw7icwo6gZeeubb3dVAInpfDwyZ8bv45Mflv7WVI5ImagK7hiB5UP56qOt5TZx=w2880-h1282-rw" class="ndfHFb-c4YZDc-HiaYvf-RJLb9c" alt="当前显示fig_1_1.png" aria-hidden="true" width="200">
</p>
Consider it as some sort of interface that tells the user to feed in some data, and then returns some other data to the user. FB only specifies what kind of data goes into it, what ought to be achieved using these input data, and what kind of data it should output. N.B. that FB itself does not do anything at all, it serves soely as an **interface**, the job is left for hardware suppliers to implement. For example, the FB called _"MC_Power"_ is used to _"enable the axes"_: 
<p align="center">
    <img src="https://lh6.googleusercontent.com/TpzC2s9elMP6On1xcZ_jeuKHoas6wxm4ZJ1slgOY2qJzky3yszIg1gTkMHv4Q9Ujd9SHaFZNrOh_uoBF3TEJ=w2100-h1380-rw" class="ndfHFb-c4YZDc-HiaYvf-RJLb9c" alt="当前显示fig_1_2.png" aria-hidden="true" width="200">
</p>
By _"enable"_ it simply means to power up the motor, so the rotor is no longer free to rotate. _MC_Power_ will take in some data, tells the actual implementation to do the powering stuff, and then output some other data. _MC_Power_ is one of the most commonly used FB, and we will take a much detailed look at it later.

### Two types of FB: Enable and Execute
There are two types of function blocks, namely the "Enable" type and the "Execute" type. Generally, these two types of FB have the following interface:
<p align="center">
    <img src="https://lh6.googleusercontent.com/_PhNW35qFPgOTRNTh0fpo6F1rBWrsu134fXYIxNf50b_s9YxgQdnCFOuEexs7V3ruuc_7gjxDmRpO0n2cixm=w2880-h1380-rw" class="ndfHFb-c4YZDc-HiaYvf-RJLb9c" alt="当前显示fig_1_3.png" aria-hidden="true">
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

### Closer look at MC_Power
The following figure shows the actuall interface of the function block _"MC_Power"_. The graphical representation used in the figure is actually a programming language called _**Sequential Function Chart(SFC)**_. SFC is often employed in big projects so as to give a lucid overview of the program structure.
<p align="center">
    <img src="https://lh4.googleusercontent.com/p1ajqmwKNEVY0IbLMEEPUlOaC3m_ZtiRNHgz-rcHkx4zQSDlFtsNsuheUHSohyPOi_MHbNMvUDFUinkr87c1=w2100-h1380-rw" class="ndfHFb-c4YZDc-HiaYvf-RJLb9c" alt="当前显示fig_1_4.png" aria-hidden="true" width="500">
</p>
This function block can also be expressed in _**Structured Text(ST)**_, which is also a frequently used programming language.
```
PROGRAM PLC_PRG
VAR
    fbPower     :   MC_Power;   // similar to 'import numpy as np' in python
```
```
fbPower(Axis := myWarpDrive_1, Enable:= TRUE, bDriveStart := TRUE, bRegulatorOn := TRUE);   // Enable Axis
```

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
