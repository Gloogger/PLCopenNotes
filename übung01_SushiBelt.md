---
layout: post
mathjax: true
title: 'Übung: Conveyor Belt of a Sushi Restaurant'
description: Contributor: Donglin Sui
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



***
The omnipotent math shows that:

$$
1+1 = 2
$$

***

Image hosting template:

```
<p align="center">
    <img src="https://drive.google.com/uc?export=view&id=" alt="Erklaerung">
</p>
```

Frequently used mathcode:
```
$$
\begin{align}
    \begin{split}
    \end{split}
\end{align}
$$

$$
\begin{bmatrix}
       
\end{bmatrix}
$$

```

<p style="text-align:center;">
<button type="button" onclick="window.location.href='#top';">Back To Top</button>
<p>
