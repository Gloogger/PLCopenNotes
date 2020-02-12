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
    <img src="https://lh6.googleusercontent.com/JgnuuQWyJcTyhCvcTKU30xrGqNItd05u8VrfOR_FJpcQc3CBUtx-qn1UXdp0rJR8l114Gsn57SZJzW_PqbcX=w2880-h1380-rw" class="ndfHFb-c4YZDc-HiaYvf-RJLb9c" alt="当前显示fig_1_2.png" aria-hidden="true" width="200">
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
2. "Error". BOOL. Tells whether there an error had occurred;
3. "ErrorID". The datatype _UINT_ means 32-bit unsigned integer, which stores an error code. User could use this error code to find more information.

The Execute type FB on the right will take in:

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
